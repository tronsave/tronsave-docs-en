---
description: Retrieve the current Energy or Bandwidth order book with available offers and prices, authenticated with a TronSave API key.
---

# Get Order Book

Retrieve the current Energy and Bandwidth order book with the available offers and their prices. Use it to inspect resource pricing and depth before placing an order.

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v2/order-book`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

## Headers

<table>
<thead>
<tr><th width="140">Name</th><th width="100">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key tied to your internal account. See <a href="../../../authentication.md">Authentication</a>.</td></tr>
</tbody>
</table>

<mark style="color:red;">*</mark> Required.

## Query parameters

<table>
<thead>
<tr><th width="220">Name</th><th width="108">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>address</code></td><td>String</td><td>Resource receiver address.</td></tr>
<tr><td><code>minDelegateAmount</code></td><td>Number</td><td>The minimum amount of Energy delegated from one provider.</td></tr>
<tr><td><code>durationSec</code></td><td>Number</td><td>Order duration in seconds.</td></tr>
<tr><td><code>resourceType</code></td><td>String</td><td><code>"ENERGY"</code> or <code>"BANDWIDTH"</code>. Default: <code>ENERGY</code>.</td></tr>
</tbody>
</table>

## Response

Each entry in `data` describes one price level: `price` is the price in SUN and `availableResourceAmount` is the resource amount available at that price.

{% tabs %}
{% tab title="200: OK" %}
```json
{
    "error": false,
    "message": "Success",
    "data": [
        {
            "price": 602,
            "availableResourceAmount": 1179
        },
        {
            "price": 650,
            "availableResourceAmount": 2409
        },
        {
            "price": 700,
            "availableResourceAmount": 5395
        },
        {
            "price": 701,
            "availableResourceAmount": 6613
        }
    ]
}
```
{% endtab %}

{% tab title="401: Missing API key" %}
```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```
{% endtab %}

{% tab title="401: Invalid API key" %}
```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```
{% endtab %}

{% tab title="400: Validation error" %}
```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "querystring must have required property 'address'"
}
```
{% endtab %}
{% endtabs %}

## Example

Query parameters:

<table>
<thead>
<tr><th width="263">Key</th><th>Value</th></tr>
</thead>
<tbody>
<tr><td><code>address</code></td><td>TFwUFWr3QV376677Z8VWXxGUAMFSrq11111</td></tr>
<tr><td><code>resourceType</code></td><td>BANDWIDTH</td></tr>
<tr><td><code>minDelegateAmount</code></td><td>1000</td></tr>
<tr><td><code>durationSec</code></td><td>86400</td></tr>
</tbody>
</table>

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.tronsave.io/v2/order-book?address=YOUR_TRON_ADDRESS" \
  -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API_URL = "https://api.tronsave.io";

const getOrderBook = async (apiKey, receiverAddress) => {
  const url = `${TRONSAVE_API_URL}/v2/order-book?address=${receiverAddress}`;
  const res = await fetch(url, {
    method: "GET",
    headers: {
      apikey: apiKey,
    },
  });
  return res.json();
};

getOrderBook("YOUR_API_KEY", "YOUR_TRON_ADDRESS").then(console.log);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"

def get_order_book(api_key: str, receiver_address: str) -> dict:
    url = f"{TRONSAVE_API_URL}/v2/order-book"
    headers = {"apikey": api_key}
    params = {"address": receiver_address}

    response = requests.get(url, headers=headers, params=params)
    return response.json()

print(get_order_book("YOUR_API_KEY", "YOUR_TRON_ADDRESS"))
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetOrderBook {
    public static void main(String[] args) throws Exception {
        String apiKey = "YOUR_API_KEY";
        String receiverAddress = "YOUR_TRON_ADDRESS";
        String url = "https://api.tronsave.io/v2/order-book?address=" + receiverAddress;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .header("apikey", apiKey)
                .GET()
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
	"fmt"
	"io"
	"net/http"
)

func main() {
	apiKey := "YOUR_API_KEY"
	receiverAddress := "YOUR_TRON_ADDRESS"
	url := "https://api.tronsave.io/v2/order-book?address=" + receiverAddress

	req, err := http.NewRequest(http.MethodGet, url, nil)
	if err != nil {
		panic(err)
	}
	req.Header.Set("apikey", apiKey)

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	body, _ := io.ReadAll(resp.Body)
	fmt.Println(string(body))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
use serde_json::Value;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let api_key = "YOUR_API_KEY";
    let receiver_address = "YOUR_TRON_ADDRESS";
    let url = format!(
        "https://api.tronsave.io/v2/order-book?address={}",
        receiver_address
    );

    let client = reqwest::blocking::Client::new();
    let resp: Value = client
        .get(&url)
        .header("apikey", api_key)
        .send()?
        .json()?;

    println!("{:#?}", resp);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* [Estimate TRX](estimate-trx.md) for a resource amount and rental duration.
* [Buy Energy (Create Order)](create-order.md) once you have picked a price level.
* Learn the difference between [Energy and Bandwidth](../../../../concepts/energy-and-bandwidth.md).
