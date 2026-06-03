---
description: Generate a signed TRX payment transaction for a resource order — either with your own code or via the TronSave Get Signed Transaction API.
---

# Get Signed Transaction

This is **Step 2** of the [Buy with Signed Transaction](README.md) flow. After [Step 1 — Estimate TRX](estimate-trx.md) returns an `estimateTrx` (the cost of the buy order in SUN), you produce a signed transfer transaction that pays that amount from the buyer's wallet to the TronSave fund address. You then submit it in [Step 3 — Create Order](create-order.md).

You can do this two ways: sign the transaction yourself ([Option 1](#option-1-write-your-own-function)), or let the TronSave API sign it for you ([Option 2](#option-2-use-the-tronsave-api)).

## Option 1: Write your own function

Using the `estimateTrx` value from [Step 1](estimate-trx.md), build a TRX transfer transaction with `transactionBuilder` that sends an amount equal to `estimateTrx` from the buyer's address to the TronSave fund address, then sign it with the buyer's private key:

```tsx
const dataSendTrx = await tronWeb.transactionBuilder.sendTrx('TRONSAVE_FUND_ADDRESS', estimate_trx, 'BUYER_ADDRESS')
const signed_tx = await tronWeb.trx.sign(dataSendTrx, 'PRIVATE_KEY');
```

{% hint style="info" %}
* `BUYER_ADDRESS` — the buyer's public address.
* `PRIVATE_KEY` — the buyer's private key.
* `TRONSAVE_FUND_ADDRESS` — the TronSave fund address (see below).
{% endhint %}

**TronSave fund address (`TRONSAVE_FUND_ADDRESS`):**

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

The resulting `signed_tx` is then passed to [Step 3 — Create Order](create-order.md).

## Option 2: Use the TronSave API

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v2/signed-tx`**

This endpoint builds and signs the transfer transaction for you, given the buyer's address, private key, and the `estimateTrx` from Step 1.

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

### Headers

| Header | Value |
| --- | --- |
| `Content-Type` | `application/json` |

### Request body

<table><thead><tr><th width="166">Field</th><th width="112">Position</th><th width="110">Type</th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>address</code></td><td>body</td><td>string</td><td>true</td><td>The buyer's public address.</td></tr><tr><td><code>privateKey</code></td><td>body</td><td>string</td><td>true</td><td>The buyer's private key.</td></tr><tr><td><code>estimateTrx</code></td><td>body</td><td>number</td><td>true</td><td>The amount of TRX to be paid, in SUN (the <code>estimateTrx</code> value from <a href="estimate-trx.md"><strong>Step 1</strong></a>).</td></tr></tbody></table>

### Request body example

```json
{
    "address": "TM6ZeEgpefyGWeMLuzSbfqTGkPv8Z65432",
    "privateKey": "YOUR_PRIVATE_KEY",
    "estimateTrx": 13500000
}
```

### Response

```json
{
    "visible": false,
    "txID": "795f8195893e8da2ef2f70fc3a1f2720f9077244c4b7b1c50c99c72fda675a32",
    "raw_data_hex": "0a02b32e2208ce1cb373c238875e4098fbd9a0eb325a68080112640a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412330a1541417ca6f74356a5d4454498e3f4856c945a04ab01121541055756f33f419278d9ea059bd2b21120e6add74818e0e5a40170b8a6d6a0eb32",
    "raw_data": {
        "contract": [
            {
                "parameter": {
                    "value": {
                        "to_address": "41055756f33f419278d9ea059bd2b21120e6add748",
                        "owner_address": "41417ca6f74356a5d4454498e3f4856c945a04ab01",
                        "amount": 2700000
                    },
                    "type_url": "type.googleapis.com/protocol.TransferContract"
                },
                "type": "TransferContract"
            }
        ],
        "ref_block_bytes": "b32e",
        "ref_block_hash": "ce1cb373c238875e",
        "expiration": 1746778095000,
        "timestamp": 1746778035000
    },
    "signature": [
        "xxxxxxxxxxxxxxxxxxx"
    ]
}
```

### Errors

This endpoint does not use an `apikey` header. Authentication for the overall flow is provided by the signed transaction itself in [Step 3 — Create Order](create-order.md); this endpoint only builds and signs the transaction. As a result it returns schema-validation errors (`400`) rather than `401` when the request is malformed.

**`400 Bad Request`** — a required field (`address`, `privateKey`, or `estimateTrx`) is missing or invalid. The `message` names the offending field:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'address'"
}
```

**`404 Not Found`** — the route/path is incorrect:

```json
{
    "message": "Route POST:/v2/... not found",
    "error": "Not Found",
    "statusCode": 404
}
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://api.tronsave.io/v2/signed-tx \
  -H "Content-Type: application/json" \
  -d '{
    "address": "YOUR_TRON_ADDRESS",
    "privateKey": "YOUR_PRIVATE_KEY",
    "estimateTrx": 13500000
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const res = await fetch("https://api.tronsave.io/v2/signed-tx", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    address: "YOUR_TRON_ADDRESS",
    privateKey: "YOUR_PRIVATE_KEY",
    estimateTrx: 13500000,
  }),
});

const data = await res.json();
console.log(data);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

res = requests.post(
    "https://api.tronsave.io/v2/signed-tx",
    headers={"Content-Type": "application/json"},
    json={
        "address": "YOUR_TRON_ADDRESS",
        "privateKey": "YOUR_PRIVATE_KEY",
        "estimateTrx": 13500000,
    },
)

print(res.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetSignedTransaction {
    public static void main(String[] args) throws Exception {
        String body = """
            {
              "address": "YOUR_TRON_ADDRESS",
              "privateKey": "YOUR_PRIVATE_KEY",
              "estimateTrx": 13500000
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.tronsave.io/v2/signed-tx"))
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString(body))
            .build();

        HttpResponse<String> response =
            client.send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
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

func main() {
	body := []byte(`{
		"address": "YOUR_TRON_ADDRESS",
		"privateKey": "YOUR_PRIVATE_KEY",
		"estimateTrx": 13500000
	}`)

	req, _ := http.NewRequest("POST", "https://api.tronsave.io/v2/signed-tx", bytes.NewBuffer(body))
	req.Header.Set("Content-Type", "application/json")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	data, _ := io.ReadAll(resp.Body)
	fmt.Println(string(data))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::blocking::Client::new();

    let res = client
        .post("https://api.tronsave.io/v2/signed-tx")
        .header("Content-Type", "application/json")
        .json(&json!({
            "address": "YOUR_TRON_ADDRESS",
            "privateKey": "YOUR_PRIVATE_KEY",
            "estimateTrx": 13500000
        }))
        .send()?;

    println!("{}", res.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
This endpoint requires the buyer's `privateKey` in the request body. Only call it from a trusted server-side environment, never from client-side code. If you prefer not to send your private key, use [Option 1](#option-1-write-your-own-function) and sign the transaction locally.
{% endhint %}

## Next steps

* [Step 1 — Estimate TRX](estimate-trx.md)
* [Step 3 — Create Order](create-order.md)
* [Buy with Signed Transaction overview](README.md)
