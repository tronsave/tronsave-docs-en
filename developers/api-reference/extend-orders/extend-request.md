---
description: Submit an extend request for an existing TronSave order — pay with your API key (internal account) or with a signed transaction — and receive an orderId.
---

# Submit Extend Request

Extend the delegates you selected in [Step 1: Get extendable delegates](get-extendable-delegates.md). Two payment methods are supported:

* **Option 1 — API key:** authorize the extension with a TronSave API key on a prefunded [internal account](../../authentication.md); no per-order on-chain signing.
* **Option 2 — Signed transaction:** sign the extension with your own private key and settle it on-chain.

Both options use the same endpoint and return an `orderId` on success.

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v2/extend-request`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

## Option 1: Extend using an API key

### Headers

<table>
<thead>
<tr><th width="140">Name</th><th width="110">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key tied to your internal account. See <a href="../../authentication.md">Authentication</a> to get your API key.</td></tr>
</tbody>
</table>

<sub>* Required.</sub>

### Request body

<table>
<thead>
<tr><th width="150">Field</th><th width="120">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>receiver</code><mark style="color:red;">*</mark></td><td>String</td><td>The address that receives the resource.</td></tr>
<tr><td><code>extendData</code><mark style="color:red;">*</mark></td><td>Array</td><td>Array of extend data. Use the response from the estimate API — see <a href="step-1-get-extendable-delegates.md">Get extendable delegates</a>.</td></tr>
<tr><td><code>resourceType</code></td><td>String</td><td><code>"ENERGY"</code> or <code>"BANDWIDTH"</code>. Default: <code>"ENERGY"</code>.</td></tr>
</tbody>
</table>

<sub>* Required.</sub>

### Request body example

```json
{
    "extendData": [
        {
            "delegator": "TFwUFWr3QV376677Z8VWXxGUAMFSSSSSSS",
            "isExtend": true,
            "extraAmount": 0,
            "extendTo": 1746702000
        },
        {
            "delegator": "TFwUFWr3QV376677Z8VWXxGUAMFFFFFFFF",
            "isExtend": true,
            "extraAmount": 0,
            "extendTo": 1746702000
        }
    ],
    "receiver": "TFwUFWr3QV376677Z8VWXxGUAMF1111111",
    "resourceType": "BANDWIDTH"
}
```

### Responses

{% tabs %}
{% tab title="201: Success" %}
Returns the order ID on success.

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "orderId": "6819da2d4d1b2aadb0d44eee"
    }
}
```
{% endtab %}

{% tab title="400: Bad Request" %}
```json
{
    "MISSING_PARAMS": "Missing some params in body",
    "INVALID_PARAMS": "Some params are invalid",
    "INTERNAL_ACCOUNT_NOT_FOUND": "internal account does not exist",
    "INTERNAL_BALANCE_ACCOUNT_TOO_LOW": "Balance is not enough",
    "SOME_DELEGATE_CANNOT_EXTEND": "This delegate order can't be extended due to some errors. Please try again later."
}
```
{% endtab %}

{% tab title="401: Unauthorized" %}
```json
{
    "API_KEY_REQUIRED": "Missing api key in headers",
    "INVALID_API_KEY": "api key not correct"
}
```
{% endtab %}

{% tab title="429: Too Many Requests" %}
```json
{
    "RATE_LIMIT": "Rate limit reached"
}
```
{% endtab %}
{% endtabs %}

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.tronsave.io/v2/extend-request" \
  -H "apikey: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "extendData": [
      {
        "delegator": "YOUR_TRON_ADDRESS",
        "isExtend": true,
        "extraAmount": 0,
        "extendTo": 1746702000
      }
    ],
    "receiver": "YOUR_TRON_ADDRESS",
    "resourceType": "BANDWIDTH"
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const sendExtendRequest = async () => {
  const url = "https://api.tronsave.io/v2/extend-request";

  // extendData comes from the Get extendable delegates response.
  const extendData = [
    {
      delegator: "YOUR_TRON_ADDRESS",
      isExtend: true,
      extraAmount: 0,
      extendTo: 1746702000,
    },
  ];

  const body = {
    extendData,
    receiver: "YOUR_TRON_ADDRESS",
    resourceType: "BANDWIDTH",
  };

  const res = await fetch(url, {
    method: "POST",
    headers: {
      apikey: "YOUR_API_KEY",
      "content-type": "application/json",
    },
    body: JSON.stringify(body),
  });

  const response = await res.json();
  // {
  //   "error": false,
  //   "message": "Success",
  //   "data": { "orderId": "6819da2d4d1b2aadb0d44eee" }
  // }
  return response;
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v2/extend-request"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
# extend_data comes from the Get extendable delegates response.
extend_data = [
    {
        "delegator": "YOUR_TRON_ADDRESS",
        "isExtend": True,
        "extraAmount": 0,
        "extendTo": 1746702000,
    },
]
body = {
    "extendData": extend_data,
    "receiver": "YOUR_TRON_ADDRESS",
    "resourceType": "BANDWIDTH",
}

response = requests.post(url, headers=headers, json=body)
print(response.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class ExtendRequest {
    public static void main(String[] args) throws Exception {
        String url = "https://api.tronsave.io/v2/extend-request";
        String body = """
            {
              "extendData": [
                {
                  "delegator": "YOUR_TRON_ADDRESS",
                  "isExtend": true,
                  "extraAmount": 0,
                  "extendTo": 1746702000
                }
              ],
              "receiver": "YOUR_TRON_ADDRESS",
              "resourceType": "BANDWIDTH"
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .header("apikey", "YOUR_API_KEY")
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
	url := "https://api.tronsave.io/v2/extend-request"
	body := []byte(`{
		"extendData": [
			{
				"delegator": "YOUR_TRON_ADDRESS",
				"isExtend": true,
				"extraAmount": 0,
				"extendTo": 1746702000
			}
		],
		"receiver": "YOUR_TRON_ADDRESS",
		"resourceType": "BANDWIDTH"
	}`)

	req, _ := http.NewRequest("POST", url, bytes.NewBuffer(body))
	req.Header.Set("apikey", "YOUR_API_KEY")
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
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "https://api.tronsave.io/v2/extend-request";
    let body = json!({
        "extendData": [
            {
                "delegator": "YOUR_TRON_ADDRESS",
                "isExtend": true,
                "extraAmount": 0,
                "extendTo": 1746702000_i64
            }
        ],
        "receiver": "YOUR_TRON_ADDRESS",
        "resourceType": "BANDWIDTH"
    });

    let client = reqwest::blocking::Client::new();
    let response = client
        .post(url)
        .header("apikey", "YOUR_API_KEY")
        .header("Content-Type", "application/json")
        .json(&body)
        .send()?;

    println!("{}", response.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Option 2: Extend using a signed transaction

This option does not require an API key. Instead, you build and sign the payment transaction with your own private key and include it as `signedTx`.

### Request body

<table>
<thead>
<tr><th width="150">Field</th><th width="170">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>receiver</code><mark style="color:red;">*</mark></td><td>String</td><td>The address that receives the resource.</td></tr>
<tr><td><code>extendData</code><mark style="color:red;">*</mark></td><td>Array</td><td>Array of extend data. Use the response from the estimate API — see <a href="step-1-get-extendable-delegates.md">Get extendable delegates</a>.</td></tr>
<tr><td><code>resourceType</code></td><td>String</td><td><code>"ENERGY"</code> or <code>"BANDWIDTH"</code>. Default: <code>"ENERGY"</code>.</td></tr>
<tr><td><code>signedTx</code></td><td>SignedTransaction</td><td>Signed transaction, as a JSON object (the <code>signedTx</code> from <a href="../buy-resources/signed-tx/get-signed-transaction.md">Get signed transaction</a>).</td></tr>
</tbody>
</table>

<sub>* Required.</sub>

{% hint style="info" %}
* To create a signed transaction, follow [Get signed transaction](../buy-resources/signed-tx/get-signed-transaction.md).
* The amount of TRX required for signing is provided in the `total_estimate_trx` field of the [Get extendable delegates](get-extendable-delegates.md) API response.
{% endhint %}

### Request body example

```json
{
    "extendData": [
        {
            "delegator": "TGGVrYaT8XoosBEXPp6dmSZkoh11223344",
            "isExtend": true,
            "extraAmount": 0,
            "extendTo": 1746403201
        }
    ],
    "receiver": "TGGVrYaT8XoosBEXPp6dmSZkoh123456",
    "resourceType": "BANDWIDTH",
    "signedTx": {
        "visible": false,
        "txID": "446eed36e31249b98b201db2e81a3825b185f1a3d8b2fea348b24fc021e58e0d",
        "raw_data": {
            "contract": [
                {
                    "parameter": {
                        "value": {
                            "amount": 5500000,
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
}
```

### Responses

{% tabs %}
{% tab title="201: Success" %}
Returns the order ID on success.

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "orderId": "6818426a65fa8ea36d119d2c"
    }
}
```
{% endtab %}

{% tab title="400: Bad Request" %}
```json
{
    "MISSING_PARAMS": "Missing some params in body",
    "INVALID_PARAMS": "Some params are invalid",
    "INTERNAL_ACCOUNT_NOT_FOUND": "internal account does not exist",
    "INTERNAL_BALANCE_ACCOUNT_TOO_LOW": "Balance is not enough",
    "SOME_DELEGATE_CANNOT_EXTEND": "This delegate order can't be extended due to some errors. Please try again later."
}
```
{% endtab %}

{% tab title="429: Too Many Requests" %}
```json
{
    "RATE_LIMIT": "Rate limit reached"
}
```
{% endtab %}
{% endtabs %}

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.tronsave.io/v2/extend-request" \
  -H "Content-Type: application/json" \
  -d '{
    "extendData": [
      {
        "delegator": "YOUR_TRON_ADDRESS",
        "isExtend": true,
        "extraAmount": 0,
        "extendTo": 1746403201
      }
    ],
    "receiver": "YOUR_TRON_ADDRESS",
    "resourceType": "BANDWIDTH",
    "signedTx": { "...": "signed transaction object" }
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const sendExtendRequest = async (extendData, signedTx) => {
  const url = "https://api.tronsave.io/v2/extend-request";

  // extendData comes from Get extendable delegates.
  // signedTx is built from your private key — see Get signed transaction.
  const body = {
    extendData,
    receiver: "YOUR_TRON_ADDRESS",
    resourceType: "BANDWIDTH",
    signedTx,
  };

  const res = await fetch(url, {
    method: "POST",
    headers: {
      "content-type": "application/json",
    },
    body: JSON.stringify(body),
  });

  const response = await res.json();
  // {
  //   "error": false,
  //   "message": "Success",
  //   "data": { "orderId": "6818426a65fa8ea36d119d2c" }
  // }
  return response;
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v2/extend-request"
headers = {
    "Content-Type": "application/json",
}
# extend_data comes from Get extendable delegates.
# signed_tx is built from your private key — see Get signed transaction.
body = {
    "extendData": extend_data,
    "receiver": "YOUR_TRON_ADDRESS",
    "resourceType": "BANDWIDTH",
    "signedTx": signed_tx,
}

response = requests.post(url, headers=headers, json=body)
print(response.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class ExtendRequestSigned {
    public static void main(String[] args) throws Exception {
        String url = "https://api.tronsave.io/v2/extend-request";
        // extendData comes from Get extendable delegates.
        // signedTx is built from your private key — see Get signed transaction.
        String body = """
            {
              "extendData": [
                {
                  "delegator": "YOUR_TRON_ADDRESS",
                  "isExtend": true,
                  "extraAmount": 0,
                  "extendTo": 1746403201
                }
              ],
              "receiver": "YOUR_TRON_ADDRESS",
              "resourceType": "BANDWIDTH",
              "signedTx": { }
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
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
	url := "https://api.tronsave.io/v2/extend-request"
	// extendData comes from Get extendable delegates.
	// signedTx is built from your private key — see Get signed transaction.
	body := []byte(`{
		"extendData": [
			{
				"delegator": "YOUR_TRON_ADDRESS",
				"isExtend": true,
				"extraAmount": 0,
				"extendTo": 1746403201
			}
		],
		"receiver": "YOUR_TRON_ADDRESS",
		"resourceType": "BANDWIDTH",
		"signedTx": {}
	}`)

	req, _ := http.NewRequest("POST", url, bytes.NewBuffer(body))
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
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "https://api.tronsave.io/v2/extend-request";
    // extendData comes from Get extendable delegates.
    // signed_tx is built from your private key — see Get signed transaction.
    let body = json!({
        "extendData": [
            {
                "delegator": "YOUR_TRON_ADDRESS",
                "isExtend": true,
                "extraAmount": 0,
                "extendTo": 1746403201_i64
            }
        ],
        "receiver": "YOUR_TRON_ADDRESS",
        "resourceType": "BANDWIDTH",
        "signedTx": {}
    });

    let client = reqwest::blocking::Client::new();
    let response = client
        .post(url)
        .header("Content-Type", "application/json")
        .json(&body)
        .send()?;

    println!("{}", response.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
To integrate with the **TRON Nile Testnet**, replace the base URL with `https://api-dev.tronsave.io`.
{% endhint %}

## Next steps

* Start the flow with [Get extendable delegates](get-extendable-delegates.md).
* Build a signed transaction with [Get signed transaction](../buy-resources/signed-tx/get-signed-transaction.md).
* Set up [Authentication](../../authentication.md) before calling the API.
