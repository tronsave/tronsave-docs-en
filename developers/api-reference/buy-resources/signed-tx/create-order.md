---
description: Submit a signed TRON transaction to create a resource order — Step 3 of the signed-transaction buy flow.
---

# Create Order

Finalize a resource purchase by submitting a signed TRON transaction. This is the final step of the [signed-transaction flow](README.md): after you [estimate the TRX required](estimate-trx.md) and [obtain a signed transaction](get-signed-transaction.md), call this endpoint to place the order.

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v2/buy-resource`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

## Headers

| Header | Value | Required |
| --- | --- | --- |
| `Content-Type` | `application/json` | Yes |

{% hint style="info" %}
This endpoint authenticates through the `signedTx` you submit — the TRX payment is signed by your own wallet — so an `apikey` header is not required. See [Authentication](../../../authentication.md).
{% endhint %}

## Request body

<table><thead><tr><th width="206">Field</th><th width="130">Type</th><th width="398">Description</th></tr></thead><tbody><tr><td><code>resourceType</code></td><td>String</td><td>"ENERGY" or "BANDWIDTH", default: ENERGY</td></tr><tr><td><code>unitPrice</code></td><td>Number</td><td>The price unit is equal to SUN.</td></tr><tr><td><code>resourceAmount</code> <mark style="color:red;">*</mark></td><td>Number</td><td>The number of resources.</td></tr><tr><td><code>receiver</code> <mark style="color:red;">*</mark></td><td>String</td><td>Resource receiving address</td></tr><tr><td><code>durationSec</code></td><td>Number</td><td>The duration of the bought resource, time unit, is in seconds. Default 259200 (3 days)</td></tr><tr><td><code>sponsor</code></td><td>String</td><td>sponsor code</td></tr><tr><td><code>signedTx</code></td><td>SignedTransaction</td><td>Signed transaction, note that it is a JSON object (the <code>signed_tx</code> produced in <a href="get-signed-transaction.md">Step 2</a>)</td></tr><tr><td><code>options</code></td><td>Object</td><td>optional</td></tr><tr><td><code>options.allowPartialFill</code></td><td>Boolean</td><td>Allow the order to be filled partially or not</td></tr><tr><td><code>options.onlyCreateWhenFulfilled</code></td><td>Boolean</td><td><p>[true] =&#x3E; order only creates when it can be fulfilled</p><p>[false] =&#x3E; order will create even if it can not be fulfilled</p><p>Default value: false</p></td></tr><tr><td><code>options.maxPriceAccepted</code></td><td>Number</td><td>Only create an order when the estimated price is less than this value.</td></tr><tr><td><code>options.preventDuplicateIncompleteOrders</code></td><td>Boolean</td><td><p>[true] =&#x3E; Only create if no <strong>uncompleted order</strong> with the same parameters exists.</p><p>[false] =&#x3E; Always create a new order, regardless of existing unfinished ones.</p><p>Default value: <strong>false</strong></p></td></tr><tr><td><code>options.minResourceDelegateRequiredAmount</code></td><td>Number</td><td>The minimum resource amount delegated by a single provider.</td></tr></tbody></table>

<mark style="color:red;">*</mark> Required field.

### Request body example

```json
{
    "resourceType": "ENERGY",
    "receiver": "TFFbwz3UpmgaPT4UudwsxbiJf63t777777",
    "durationSec": 3600,
    "resourceAmount": 32000,
    "unitPrice": 80,
    "options": {
        "allowPartialFill": true,
        "onlyCreateWhenFulfilled": true,
        "preventDuplicateIncompleteOrders": false,
        "maxPriceAccepted": 100,
        "minResourceDelegateRequiredAmount": 100000
    },
    "signedTx": {
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
}
```

## Response

### Success

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "orderId": "6818426a65fa8ea36d119d2c"
    }
}
```

### Errors

This endpoint authenticates through the `signedTx`, so it does **not** return a `401`/API-key error. Invalid or missing input is rejected with a `400` schema-validation error before the order is created.

**`400 Bad Request`** — a required field is missing or invalid. The `message` names the offending property (here, `receiver`):

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'receiver'"
}
```

The order may also fail with a business-logic error returned in the success envelope (`error: true`). For example, when the order cannot be fulfilled or the account balance is insufficient:

```json
{
    "error": true,
    "message": "<error message>"
}
```

<!-- [NEEDS CONFIRMATION: exact business-logic error messages (e.g. balance too low / cannot be fulfilled) for this endpoint are not provided by the source.] -->

## Request examples

Replace `YOUR_API_KEY` and `YOUR_TRON_ADDRESS` where applicable. The `signedTx` object below is abbreviated — pass the full signed transaction obtained in [Step 2](get-signed-transaction.md).

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.tronsave.io/v2/buy-resource" \
  -H "Content-Type: application/json" \
  -d '{
    "resourceType": "ENERGY",
    "receiver": "YOUR_TRON_ADDRESS",
    "durationSec": 3600,
    "resourceAmount": 32000,
    "unitPrice": 80,
    "options": {
        "allowPartialFill": true,
        "onlyCreateWhenFulfilled": true,
        "preventDuplicateIncompleteOrders": false,
        "maxPriceAccepted": 100,
        "minResourceDelegateRequiredAmount": 100000
    },
    "signedTx": {
        "visible": false,
        "txID": "795f8195893e8da2ef2f70fc3a1f2720f9077244c4b7b1c50c99c72fda675a32",
        "raw_data_hex": "0a02b32e...",
        "raw_data": {},
        "signature": ["xxxxxxxxxxxxxxxxxxx"]
    }
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API_URL = "https://api.tronsave.io";

const createOrder = async (resourceAmount, signedTx, receiverAddress, unitPrice, durationSec, options) => {
  const url = `${TRONSAVE_API_URL}/v2/buy-resource`;
  const body = {
    resourceType: "ENERGY",
    resourceAmount,
    unitPrice,
    receiver: receiverAddress,
    durationSec,
    signedTx,
    options,
  };

  const res = await fetch(url, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(body),
  });

  // Example response:
  // { "error": false, "message": "Success", "data": { "orderId": "6809fdb7b9ba217a41d726fd" } }
  return res.json();
};

createOrder(
  32000,
  signedTx, // the signed transaction from Step 2
  "YOUR_TRON_ADDRESS",
  80,
  3600,
  { allowPartialFill: true, onlyCreateWhenFulfilled: true, maxPriceAccepted: 100 }
).then(console.log);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"


def create_order(resource_amount, signed_tx, receiver_address, unit_price, duration_sec, options):
    url = f"{TRONSAVE_API_URL}/v2/buy-resource"
    body = {
        "resourceType": "ENERGY",
        "resourceAmount": resource_amount,
        "unitPrice": unit_price,
        "receiver": receiver_address,
        "durationSec": duration_sec,
        "signedTx": signed_tx,
        "options": options,
    }

    response = requests.post(url, json=body)
    return response.json()


if __name__ == "__main__":
    result = create_order(
        resource_amount=32000,
        signed_tx=signed_tx,  # the signed transaction from Step 2
        receiver_address="YOUR_TRON_ADDRESS",
        unit_price=80,
        duration_sec=3600,
        options={"allowPartialFill": True, "onlyCreateWhenFulfilled": True, "maxPriceAccepted": 100},
    )
    print(result)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class CreateOrder {
    static final String TRONSAVE_API_URL = "https://api.tronsave.io";

    public static void main(String[] args) throws Exception {
        // signedTx is the JSON object obtained in Step 2.
        String signedTx = "{\"visible\":false,\"txID\":\"795f8195...\",\"raw_data_hex\":\"0a02b32e...\",\"raw_data\":{},\"signature\":[\"xxxxxxxxxxxxxxxxxxx\"]}";

        String body = """
            {
              "resourceType": "ENERGY",
              "receiver": "YOUR_TRON_ADDRESS",
              "durationSec": 3600,
              "resourceAmount": 32000,
              "unitPrice": 80,
              "options": {
                "allowPartialFill": true,
                "onlyCreateWhenFulfilled": true,
                "maxPriceAccepted": 100
              },
              "signedTx": %s
            }
            """.formatted(signedTx);

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(TRONSAVE_API_URL + "/v2/buy-resource"))
                .header("Content-Type", "application/json")
                .POST(HttpRequest.BodyPublishers.ofString(body))
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
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

const tronsaveAPIURL = "https://api.tronsave.io"

func main() {
	// signedTx is the JSON object obtained in Step 2.
	body := []byte(`{
        "resourceType": "ENERGY",
        "receiver": "YOUR_TRON_ADDRESS",
        "durationSec": 3600,
        "resourceAmount": 32000,
        "unitPrice": 80,
        "options": {
            "allowPartialFill": true,
            "onlyCreateWhenFulfilled": true,
            "maxPriceAccepted": 100
        },
        "signedTx": {
            "visible": false,
            "txID": "795f8195...",
            "raw_data_hex": "0a02b32e...",
            "raw_data": {},
            "signature": ["xxxxxxxxxxxxxxxxxxx"]
        }
    }`)

	req, err := http.NewRequest(http.MethodPost, tronsaveAPIURL+"/v2/buy-resource", bytes.NewBuffer(body))
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
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
// Cargo.toml:
//   reqwest = { version = "0.12", features = ["blocking", "json"] }
//   serde_json = "1"

use serde_json::json;

const TRONSAVE_API_URL: &str = "https://api.tronsave.io";

fn main() -> Result<(), Box<dyn std::error::Error>> {
    // signed_tx is the JSON object obtained in Step 2.
    let signed_tx = json!({
        "visible": false,
        "txID": "795f8195...",
        "raw_data_hex": "0a02b32e...",
        "raw_data": {},
        "signature": ["xxxxxxxxxxxxxxxxxxx"]
    });

    let body = json!({
        "resourceType": "ENERGY",
        "receiver": "YOUR_TRON_ADDRESS",
        "durationSec": 3600,
        "resourceAmount": 32000,
        "unitPrice": 80,
        "options": {
            "allowPartialFill": true,
            "onlyCreateWhenFulfilled": true,
            "maxPriceAccepted": 100
        },
        "signedTx": signed_tx
    });

    let client = reqwest::blocking::Client::new();
    let resp = client
        .post(format!("{TRONSAVE_API_URL}/v2/buy-resource"))
        .header("Content-Type", "application/json")
        .json(&body)
        .send()?;

    println!("{}", resp.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* [Estimate TRX](estimate-trx.md) — recalculate cost before placing another order.
* [Get Signed Transaction](get-signed-transaction.md) — produce the `signedTx` this endpoint requires.
* [Buy with Signed Transaction overview](README.md) — the full three-step flow.
