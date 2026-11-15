---
description: Legacy v0 flow for buying Energy with a signed TRON transaction — estimate TRX, generate a signed transfer, and create the order.
---

# Buy with Signed Transaction (v0)

{% hint style="warning" %}
**Legacy API.** This page documents the v0 signed-transaction flow under the `/v0/` base path. New integrations should use the current API. See [Buy with Signed Transaction (v2)](../../api-reference/buy-resources/signed-tx/README.md).
{% endhint %}

The signed-transaction flow lets you buy Energy by paying TRX directly from your own wallet — no API key required, since the TRX transfer is signed with your private key. There are three steps:

1. [Estimate TRX](#step-1-estimate-trx) — calculate the TRX needed for the desired amount and rental duration.
2. [Get a signed transaction](#step-2-get-a-signed-transaction) — sign a TRX transfer to the TronSave fund address, either yourself or via the API.
3. [Create an order](#step-3-create-an-order) — submit the signed transaction to place the buy order.

{% hint style="info" %}
**Before you start:** make sure you have the wallet's private key and that the wallet holds enough TRX to cover the estimated cost.
{% endhint %}

The mainnet base URL is `https://api.tronsave.io`. For testing on the TRON Nile testnet, use `https://api-dev.tronsave.io`. See [Environments](../../environments.md).

| Step | Method | Mainnet | Testnet (Nile) |
| --- | --- | --- | --- |
| Estimate TRX | `POST` | `https://api.tronsave.io/v0/estimate-trx` | `https://api-dev.tronsave.io/v0/estimate-trx` |
| Get Signed Transaction | `POST` | `https://api.tronsave.io/v0/signed-tx` | `https://api-dev.tronsave.io/v0/signed-tx` |
| Create Order | `POST` | `https://api.tronsave.io/v0/buy-energy` | `https://api-dev.tronsave.io/v0/buy-energy` |

---

## Step 1: Estimate TRX

Calculate the TRX needed for a given resource amount and rental duration. The response gives you the `unit_price` and the `estimate_trx` you must transfer in Step 2.

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/estimate-trx`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

### Headers

| Header | Value | Required |
| --- | --- | --- |
| `Content-Type` | `application/json` | Yes |

### Request params

<table><thead><tr><th width="174">Field</th><th width="94">Position</th><th width="92">Type</th><th width="100">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>amount</code></td><td>body</td><td>number</td><td>true</td><td>The number of resources</td></tr><tr><td><code>buy_energy_type</code></td><td>body</td><td>string, number</td><td>true</td><td><p>"FAST", "MEDIUM", "SLOW" or number:</p><p><br>-"FAST": If market ready to fill = 100%, FAST = MEDIUM. If the market ready to fill &#x3C; 100%, FAST = MEDIUM + 10. If market ready to fill = 0%, FAST = SLOW + 20.</p><p></p><p>-"MEDIUM": The lowest price for the maximum market fill for this order. If market ready to fill = 0%, MEDIUM = SLOW + 10.</p><p></p><p>-"SLOW": The lowest price that can be set for this order.</p><p></p><p>-If the price is a number, the price unit is equal to SUN</p></td></tr><tr><td><code>duration_millisec</code></td><td>body</td><td>number</td><td>true</td><td>The duration of the bought resource, time unit is equal to millisecond.</td></tr><tr><td><code>request_address</code></td><td>body</td><td>string</td><td>false</td><td>The address of requester.</td></tr><tr><td><code>target_address</code></td><td>body</td><td>string</td><td>false</td><td>The address of receiver resource.</td></tr><tr><td><code>is_partial</code></td><td>body</td><td>boolean</td><td>false</td><td>Allow the order to be filled partially or not.</td></tr></tbody></table>

### Request body example

```json
{
    "amount": 100000,
    "buy_energy_type": "MEDIUM",
    "duration_millisec": 259200000
}
```

### Responses

<table><thead><tr><th width="185">Field</th><th width="171">Type</th><th width="158">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>unit_price</code></td><td>number</td><td>true</td><td>price in SUN of energy that fit with your <code>buy_energy_type</code></td></tr><tr><td><code>duration_millisec</code></td><td>number</td><td>true</td><td></td></tr><tr><td><code>available_energy</code></td><td>number</td><td>true</td><td>total amount available energy on TronSave market that match with <code>unit_price</code></td></tr><tr><td><code>estimate_trx</code></td><td>number</td><td>true</td><td>estimate total trx value will pay for buy all <code>available_energy</code> with price is <code>unit_price</code> in <code>duration_millisec</code></td></tr></tbody></table>

#### Success

```json
{
    "unit_price": 45,
    "duration_millisec": 259200000,
    "available_energy": 4298470,
    "estimate_trx": 13500000
}
```

The `estimate_trx` value is returned in SUN (1 TRX = 1,000,000 SUN).

#### Error

This endpoint does not require an `apikey` header — the signed-transaction flow authenticates through the wallet-signed TRX transfer, so no authentication error applies here. A request with a missing or invalid field returns a `400 Bad Request` whose `message` names the offending field:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'amount'"
}
```

---

## Step 2: Get a signed transaction

You need a signed TRX transfer of `estimate_trx` SUN from the buyer's address to the TronSave fund address. You can build it yourself, or let the API do it.

### TronSave fund address

{% tabs %}
{% tab title="MAINNET" %}
```
TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S
```
{% endtab %}

{% tab title="Nile (for testing)" %}
{% hint style="warning" %}
This fund address is for testing purposes only on the TRON Nile network.
{% endhint %}

```
TATT1UzHRikft98bRFqApFTsaSw73ycfoS
```
{% endtab %}
{% endtabs %}

### Option 1: Write your own function

Using `tronWeb`, build a `sendTrx` transfer for `estimate_trx` (from Step 1) and sign it with the buyer's private key:

```javascript
const dataSendTrx = await tronWeb.transactionBuilder.sendTrx('TRONSAVE_FUND_ADDRESS', estimate_trx, 'BUYER_ADDRESS')
const signed_tx = await tronWeb.trx.sign(dataSendTrx, 'PRIVATE_KEY');
```

{% hint style="info" %}
* `BUYER_ADDRESS` is the buyer's public address.
* `PRIVATE_KEY` is the buyer's private key.
* `TRONSAVE_FUND_ADDRESS` is the TronSave fund address shown above.
{% endhint %}

### Option 2: Use the Get Signed Transaction API

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/signed-tx`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

#### Headers

| Header | Value | Required |
| --- | --- | --- |
| `Content-Type` | `application/json` | Yes |

#### Request params

<table><thead><tr><th width="166">Field</th><th width="112">Position</th><th width="110">Type</th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>address</code></td><td>body</td><td>string</td><td>true</td><td>The buyer's public address</td></tr><tr><td><code>private_key</code></td><td>body</td><td>string</td><td>true</td><td>The buyer's private key</td></tr><tr><td><code>estimate_trx</code></td><td>body</td><td>number</td><td>true</td><td>The amount of TRX to be paid is calculated in SUN. (<code>estimate_trx</code> from <a href="#step-1-estimate-trx">Step 1</a>)</td></tr></tbody></table>

#### Request body example

```json
{
    "address": "TM6ZeEgpefyGWeMLuzSbfqTGkPv8Z65432",
    "private_key": "{{yourprivateKey}}",
    "estimate_trx": 13500000
}
```

#### Responses

##### Success

```json
{
    "visible": false,
    "txID": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
    "raw_data": {
        "contract": [
            {
                "parameter": {
                    "value": {
                        "amount": 13500000,
                        "owner_address": "417a0d868d1418c9038584af1252f85d486502eec0",
                        "to_address": "41055756f33f419278d9ea059bd2b21120e6add748"
                    },
                    "type_url": "type.googleapis.com/protocol.TransferContract"
                },
                "type": "TransferContract"
            }
        ],
        "ref_block_bytes": "0713",
        "ref_block_hash": "6c5f7686f4176139",
        "expiration": 1691465106000,
        "timestamp": 1691465046758
    },
    "raw_data_hex": "0a02071322086c5f7686f417613940d084b5999d315a68080112640a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412330a15417a0d868d1418c9038584af1252f85d486502eec0121541055756f33f419278d9ea059bd2b21120e6add74818e0fcb70670e6b5b1999d31",
    "signature": ["xxxxxxxxx"]
}
```

##### Error

No `apikey` header is required for this endpoint. A request with a missing or invalid field returns a `400 Bad Request` whose `message` names the offending field:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'estimate_trx'"
}
```

---

## Step 3: Create an order

Submit the signed transaction (from Step 2) together with the order parameters to place the Energy purchase.

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/buy-energy`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

### Headers

| Header | Value | Required |
| --- | --- | --- |
| `Content-Type` | `application/json` | Yes |

{% hint style="info" %}
This endpoint authenticates through the `signed_tx` you submit — the TRX payment is signed by your own wallet — so an `apikey` header is not required.
{% endhint %}

### Request params

<table><thead><tr><th width="221">Field</th><th width="105">Type</th><th width="91">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>resource_type</code></td><td>string</td><td>true</td><td>"ENERGY"</td></tr><tr><td><code>unit_price</code></td><td>number</td><td>true</td><td>Price unit is equal to SUN.</td></tr><tr><td><code>allow_partial_fill</code></td><td>boolean</td><td>true</td><td>Allow the order to be filled partially or not</td></tr><tr><td><code>target_address</code></td><td>string</td><td>true</td><td>Resource receiving address</td></tr><tr><td><code>duration_millisec</code></td><td>number</td><td>true</td><td>The duration of the bought resource, time unit is equal to millisec.</td></tr><tr><td><code>tx_id</code></td><td>string</td><td>true</td><td>Transaction ID</td></tr><tr><td><code>signed_tx</code></td><td>SignedTransaction</td><td>true</td><td>Signed transaction, note that it is a JSON object (the <code>signed_tx</code> from <a href="#step-2-get-a-signed-transaction">Step 2</a>)</td></tr><tr><td><code>only_create_when_fulfilled</code></td><td>Boolean</td><td>false</td><td><p>[true] =&#x3E; order only create when it can be fulfilled</p><p>[false] =&#x3E; order will create even it can not be fulfilled</p><p>Default value: false</p></td></tr><tr><td><code>max_price_accepted</code></td><td>Number</td><td>false</td><td>Only create order when the estimate price less than this value.</td></tr><tr><td><code>add_order_incomplete</code></td><td>Boolean</td><td>false</td><td><p>[true] =&#x3E; order only create when there has no same parameters order is not complete in order list</p><p>[false] =&#x3E; order will create even there has no same parameters order is not complete in order list</p><p>Default value: false</p></td></tr></tbody></table>

### Request body example

```json
{
    "resource_type": "ENERGY",
    "unit_price": 45,
    "allow_partial_fill": true,
    "target_address": "TM6ZeEgpefyGWeMLuzSbfqTGkPv8Z6Jm4X",
    "duration_millisec": 259200000,
    "tx_id": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
    "signed_tx": {
        "visible": false,
        "txID": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
        "raw_data": {
            "contract": [
                {
                    "parameter": {
                        "value": {
                            "amount": 13500000,
                            "owner_address": "417a0d868d1418c9038584af1252f85d486502eec0",
                            "to_address": "41055756f33f419278d9ea059bd2b21120e6add748"
                        },
                        "type_url": "type.googleapis.com/protocol.TransferContract"
                    },
                    "type": "TransferContract"
                }
            ],
            "ref_block_bytes": "0713",
            "ref_block_hash": "6c5f7686f4176139",
            "expiration": 1691465106000,
            "timestamp": 1691465046758
        },
        "raw_data_hex": "0a02071322086c5f7686f417613940d084b5999d315a68080112640a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412330a15417a0d868d1418c9038584af1252f85d486502eec0121541055756f33f419278d9ea059bd2b21120e6add74818e0fcb70670e6b5b1999d31",
        "signature": ["xxxxxxxxx"]
    },
    "only_create_when_fulfilled": false,
    "max_price_accepted": 200,
    "add_order_incomplete": false
}
```

### Responses

#### Success

```json
{
    "message": "651d2306e55c073f6ca0992e"
}
```

The `message` value is the `order_id` of the created order.

#### Error

This endpoint authenticates through the submitted `signed_tx`, so no `apikey` header and no authentication error apply. A request with a missing or invalid field returns a `400 Bad Request` whose `message` names the offending field:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'signed_tx'"
}
```

<!-- [NEEDS CONFIRMATION: business-logic 400 messages specific to the buy-energy order (e.g. insufficient TRX in the signed transfer, or order cannot be fulfilled) were not captured live] -->

---

## Request examples

The examples below show the full flow: estimate, sign the TRX transfer to the fund address, then create the order. Replace `YOUR_TRON_ADDRESS`, the private key, and the fund address as needed. Signing the TRX transfer (Step 2, Option 1) typically uses a TRON library; the cURL example uses the API (Step 2, Option 2) instead.

{% tabs %}
{% tab title="cURL" %}
```bash
# Step 1: Estimate TRX
curl -X POST https://api.tronsave.io/v0/estimate-trx \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 100000,
    "buy_energy_type": "MEDIUM",
    "duration_millisec": 259200000
  }'

# Step 2: Get a signed transaction (Option 2)
curl -X POST https://api.tronsave.io/v0/signed-tx \
  -H "Content-Type: application/json" \
  -d '{
    "address": "YOUR_TRON_ADDRESS",
    "private_key": "YOUR_PRIVATE_KEY",
    "estimate_trx": 13500000
  }'

# Step 3: Create the order (use the signed_tx returned above)
curl -X POST https://api.tronsave.io/v0/buy-energy \
  -H "Content-Type: application/json" \
  -d '{
    "resource_type": "ENERGY",
    "unit_price": 45,
    "allow_partial_fill": true,
    "target_address": "YOUR_TRON_ADDRESS",
    "duration_millisec": 259200000,
    "tx_id": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
    "signed_tx": { "visible": false, "txID": "446eed36...", "raw_data": {}, "raw_data_hex": "0a020713...", "signature": ["xxxxxxxxx"] },
    "only_create_when_fulfilled": false,
    "max_price_accepted": 200,
    "add_order_incomplete": false
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// Requires tronweb for signing the TRX transfer.
const TRONSAVE_API_URL = "https://api.tronsave.io";
const TRONSAVE_FUND_ADDRESS = "TWZEhq5JuUVvGtutNgnRBATbF8BnHGyn4S";

// Step 1: Estimate TRX
const estimateRes = await fetch(`${TRONSAVE_API_URL}/v0/estimate-trx`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    amount: 100000,
    buy_energy_type: "MEDIUM",
    duration_millisec: 259200000,
  }),
});
const { unit_price, duration_millisec, estimate_trx } = await estimateRes.json();

// Step 2 (Option 1): Sign the TRX transfer to the fund address
const dataSendTrx = await tronWeb.transactionBuilder.sendTrx(
  TRONSAVE_FUND_ADDRESS,
  estimate_trx,
  "YOUR_TRON_ADDRESS"
);
const signed_tx = await tronWeb.trx.sign(dataSendTrx, "YOUR_PRIVATE_KEY");

// Step 3: Create the order
const orderRes = await fetch(`${TRONSAVE_API_URL}/v0/buy-energy`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    resource_type: "ENERGY",
    unit_price,
    allow_partial_fill: true,
    target_address: "YOUR_TRON_ADDRESS",
    duration_millisec,
    tx_id: signed_tx.txID,
    signed_tx,
    only_create_when_fulfilled: false,
    max_price_accepted: 200,
    add_order_incomplete: false,
  }),
});
console.log(await orderRes.text());
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"

# Step 1: Estimate TRX
estimate = requests.post(
    f"{TRONSAVE_API_URL}/v0/estimate-trx",
    json={
        "amount": 100000,
        "buy_energy_type": "MEDIUM",
        "duration_millisec": 259200000,
    },
).json()
unit_price = estimate["unit_price"]
duration_millisec = estimate["duration_millisec"]
estimate_trx = estimate["estimate_trx"]

# Step 2 (Option 2): Get a signed transaction from the API
signed_tx = requests.post(
    f"{TRONSAVE_API_URL}/v0/signed-tx",
    json={
        "address": "YOUR_TRON_ADDRESS",
        "private_key": "YOUR_PRIVATE_KEY",
        "estimate_trx": estimate_trx,
    },
).json()

# Step 3: Create the order
order = requests.post(
    f"{TRONSAVE_API_URL}/v0/buy-energy",
    json={
        "resource_type": "ENERGY",
        "unit_price": unit_price,
        "allow_partial_fill": True,
        "target_address": "YOUR_TRON_ADDRESS",
        "duration_millisec": duration_millisec,
        "tx_id": signed_tx["txID"],
        "signed_tx": signed_tx,
        "only_create_when_fulfilled": False,
        "max_price_accepted": 200,
        "add_order_incomplete": False,
    },
).json()
print(order)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class BuyWithSignedTx {
    static final String TRONSAVE_API_URL = "https://api.tronsave.io";

    static String post(HttpClient client, String path, String body) throws Exception {
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(TRONSAVE_API_URL + path))
                .header("Content-Type", "application/json")
                .POST(HttpRequest.BodyPublishers.ofString(body))
                .build();
        return client.send(request, HttpResponse.BodyHandlers.ofString()).body();
    }

    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();

        // Step 1: Estimate TRX
        String estimate = post(client, "/v0/estimate-trx", """
            {
              "amount": 100000,
              "buy_energy_type": "MEDIUM",
              "duration_millisec": 259200000
            }
            """);
        System.out.println(estimate);

        // Step 2 (Option 2): Get a signed transaction from the API
        String signedTx = post(client, "/v0/signed-tx", """
            {
              "address": "YOUR_TRON_ADDRESS",
              "private_key": "YOUR_PRIVATE_KEY",
              "estimate_trx": 13500000
            }
            """);
        System.out.println(signedTx);

        // Step 3: Create the order (embed the signedTx JSON from Step 2)
        String order = post(client, "/v0/buy-energy", """
            {
              "resource_type": "ENERGY",
              "unit_price": 45,
              "allow_partial_fill": true,
              "target_address": "YOUR_TRON_ADDRESS",
              "duration_millisec": 259200000,
              "tx_id": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
              "signed_tx": %s,
              "only_create_when_fulfilled": false,
              "max_price_accepted": 200,
              "add_order_incomplete": false
            }
            """.formatted(signedTx));
        System.out.println(order);
    }
}
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
	"bytes"
	"fmt"
	"io"
	"net/http"
)

const tronsaveAPIURL = "https://api.tronsave.io"

func post(path string, body []byte) string {
	req, err := http.NewRequest(http.MethodPost, tronsaveAPIURL+path, bytes.NewBuffer(body))
	if err != nil {
		panic(err)
	}
	req.Header.Set("Content-Type", "application/json")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	out, _ := io.ReadAll(resp.Body)
	return string(out)
}

func main() {
	// Step 1: Estimate TRX
	estimate := post("/v0/estimate-trx", []byte(`{
		"amount": 100000,
		"buy_energy_type": "MEDIUM",
		"duration_millisec": 259200000
	}`))
	fmt.Println(estimate)

	// Step 2 (Option 2): Get a signed transaction from the API
	signedTx := post("/v0/signed-tx", []byte(`{
		"address": "YOUR_TRON_ADDRESS",
		"private_key": "YOUR_PRIVATE_KEY",
		"estimate_trx": 13500000
	}`))
	fmt.Println(signedTx)

	// Step 3: Create the order (embed the signed_tx JSON from Step 2)
	order := post("/v0/buy-energy", []byte(`{
		"resource_type": "ENERGY",
		"unit_price": 45,
		"allow_partial_fill": true,
		"target_address": "YOUR_TRON_ADDRESS",
		"duration_millisec": 259200000,
		"tx_id": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
		"signed_tx": {},
		"only_create_when_fulfilled": false,
		"max_price_accepted": 200,
		"add_order_incomplete": false
	}`))
	fmt.Println(order)
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
// Cargo.toml:
//   reqwest = { version = "0.12", features = ["blocking", "json"] }
//   serde_json = "1"

use reqwest::blocking::Client;
use serde_json::{json, Value};

const TRONSAVE_API_URL: &str = "https://api.tronsave.io";

fn post(client: &Client, path: &str, body: &Value) -> Value {
    client
        .post(format!("{TRONSAVE_API_URL}{path}"))
        .header("Content-Type", "application/json")
        .json(body)
        .send()
        .unwrap()
        .json()
        .unwrap()
}

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    // Step 1: Estimate TRX
    let estimate = post(&client, "/v0/estimate-trx", &json!({
        "amount": 100000,
        "buy_energy_type": "MEDIUM",
        "duration_millisec": 259200000
    }));
    println!("{estimate}");

    // Step 2 (Option 2): Get a signed transaction from the API
    let signed_tx = post(&client, "/v0/signed-tx", &json!({
        "address": "YOUR_TRON_ADDRESS",
        "private_key": "YOUR_PRIVATE_KEY",
        "estimate_trx": estimate["estimate_trx"]
    }));
    println!("{signed_tx}");

    // Step 3: Create the order
    let order = post(&client, "/v0/buy-energy", &json!({
        "resource_type": "ENERGY",
        "unit_price": estimate["unit_price"],
        "allow_partial_fill": true,
        "target_address": "YOUR_TRON_ADDRESS",
        "duration_millisec": estimate["duration_millisec"],
        "tx_id": signed_tx["txID"],
        "signed_tx": signed_tx,
        "only_create_when_fulfilled": false,
        "max_price_accepted": 200,
        "add_order_incomplete": false
    }));
    println!("{order}");

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* Migrate to the current API: [Buy with Signed Transaction (v2)](../../api-reference/buy-resources/signed-tx/README.md).
* [Authentication](../../authentication.md) and [Environments](../../environments.md).
* [Glossary](../../../concepts/glossary.md) for Energy, Bandwidth, TRX, and SUN definitions.
