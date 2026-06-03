---
description: >-
  Query an order to find which delegates are eligible for extension, how much
  TRX it will cost, and the extendData payload you pass to the extend request in
  step 2.
---

# Get Extendable Delegates

This is step 1 of the v2 extend flow. Given a `receiver` address and a target `extendTo` time, the endpoint returns the delegates that can still be extended, the estimated TRX cost, and an `extendData` array. Use that `extendData` to build the payload for [Submit extend request](extend-request.md).

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v2/get-extendable-delegates`**

{% hint style="info" %}
Rate limit: **1** request per **1** second.
{% endhint %}

## Headers

<table><thead><tr><th width="140">Name</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key tied to your internal account. See <a href="../../authentication.md">Authentication</a> to get your API key.</td></tr></tbody></table>

<sub><mark style="color:red;">\*<mark style="color:red;"></sub> <sub></sub><sub>Required.</sub>

## Request body

<table><thead><tr><th width="200">Field</th><th width="120">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>extendTo</code><mark style="color:red;">*</mark></td><td>String</td><td>Time in seconds you want to extend to.</td></tr><tr><td><code>receiver</code><mark style="color:red;">*</mark></td><td>String</td><td>The address that received the resource delegate.</td></tr><tr><td><code>requester</code></td><td>String</td><td>The address of the requester. If not provided, the requester is taken from the <strong>API key</strong>.</td></tr><tr><td><code>resourceType</code></td><td>String</td><td><code>"ENERGY"</code> or <code>"BANDWIDTH"</code>. Default: <code>"ENERGY"</code>.</td></tr><tr><td><code>maxPriceAccepted</code></td><td>Number</td><td>The maximum price you want to pay to extend.</td></tr></tbody></table>

<sub><mark style="color:red;">\*<mark style="color:red;"></sub> <sub></sub><sub>Required.</sub>

### Request body example

```json
{
    "extendTo": 1728704969000,
    "maxPriceAccepted": 165,
    "receiver": "TFwUFWr3QV376677Z8VWXxGUAMF11111111",
    "resourceType": "ENERGY"
}
```

## Responses

{% tabs %}
{% tab title="200: Success" %}
```javascript
{
    "extendOrderBook": [
        {
            "price": 133,
            "value": 64319
        },
        ...
    ], // Overview of the extendable resource amount at every single price
    "totalDelegateAmount": 64319,
    // Total current delegate of the receiver address in TronSave
    "totalAvailableExtendAmount": 64319,
    // Total available delegates of the receiver address in TronSave
    "totalEstimateTrx": 8554427,
    // Estimated TRX payout if using the extendData below to create the extend request
    "yourBalance": 20000000,
    // API key's internal balance
    "isAbleToExtend": true,
    // Compares internal balance and totalEstimateTrx
    "extendData": [
        {
            "delegator": "TMN2uTdy6rQYaTm4A5g732kHRf72222222",
            "isExtend": true,
            "extraAmount": 0,
            "extendTo": 1728459019
        }
    ]
    // extendData that is used to create the extend request in Step 2
}
```
{% endtab %}

{% tab title="400: Bad Request" %}
```json
{
    "MISSING_PARAMS": "Missing some params in body",
    "INVALID_PARAMS": "Some params are invalid"
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

### Success response example

```json
{
    "extendOrderBook": [
        {
            "price": 108,
            "value": 100000
        },
        {
            "price": 122,
            "value": 200000
        }
    ],
    "totalDelegateAmount": 500000,
    "totalAvailableExtendAmount": 300000,
    "totalEstimateTrx": 24426224,
    "isAbleToExtend": true,
    "yourBalance": 37780396,
    "extendData": [
        {
            "delegator": "TQBV7xU489Rq8ZCsYi72zBhJM44444444",
            "isExtend": true,
            "extraAmount": 0,
            "extendTo": 1728704969000
        },
        {
            "delegator": "TMN2uTdy6rQYaTm4A5g732kHR333333333",
            "isExtend": true,
            "extraAmount": 0,
            "extendTo": 1728704969000
        }
    ]
}
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.tronsave.io/v2/get-extendable-delegates" \
  -H "apikey: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "extendTo": 1728704969000,
    "maxPriceAccepted": 165,
    "receiver": "YOUR_TRON_ADDRESS",
    "resourceType": "ENERGY"
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const getExtendableDelegates = async () => {
  const url = "https://api.tronsave.io/v2/get-extendable-delegates";
  const body = {
    extendTo: 1728704969000, // time in seconds you want to extend to
    receiver: "YOUR_TRON_ADDRESS", // the address that received the resource delegate
    maxPriceAccepted: 165, // optional. Max price you want to pay to extend
    resourceType: "ENERGY", // "ENERGY" or "BANDWIDTH". Optional. Default: "ENERGY"
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
  // response.extendData is used to create the extend request in Step 2
  return response;
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v2/get-extendable-delegates"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "extendTo": 1728704969000,
    "receiver": "YOUR_TRON_ADDRESS",
    "maxPriceAccepted": 165,
    "resourceType": "ENERGY",
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

public class GetExtendableDelegates {
    public static void main(String[] args) throws Exception {
        String url = "https://api.tronsave.io/v2/get-extendable-delegates";
        String body = """
            {
              "extendTo": 1728704969000,
              "receiver": "YOUR_TRON_ADDRESS",
              "maxPriceAccepted": 165,
              "resourceType": "ENERGY"
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
	url := "https://api.tronsave.io/v2/get-extendable-delegates"
	body := []byte(`{
		"extendTo": 1728704969000,
		"receiver": "YOUR_TRON_ADDRESS",
		"maxPriceAccepted": 165,
		"resourceType": "ENERGY"
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
    let url = "https://api.tronsave.io/v2/get-extendable-delegates";
    let body = json!({
        "extendTo": 1728704969000_i64,
        "receiver": "YOUR_TRON_ADDRESS",
        "maxPriceAccepted": 165,
        "resourceType": "ENERGY"
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

{% hint style="info" %}
To integrate with the **TRON Nile Testnet**, replace the base URL with `https://api-dev.tronsave.io`.
{% endhint %}

## Next steps

* Pass the `extendData` from this response into [Submit extend request](extend-request.md) to complete the extension.
* Set up [Authentication](../../authentication.md) before calling the API.
* Review [Energy and Bandwidth](../../../concepts/energy-and-bandwidth.md) and [Order Types](../../../concepts/order-types.md).
