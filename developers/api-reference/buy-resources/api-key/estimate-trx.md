---
description: Estimate the TRX required to buy a given amount of Energy or Bandwidth for a chosen rental duration, using your TronSave API key.
---

# Estimate TRX

Use this endpoint to calculate how much TRX an order will cost before you place it. Pass the resource amount, rental duration, and a unit price (or a speed tier), and TronSave returns the resolved unit price, the estimated TRX, and how much of the requested resource is currently available.

## Endpoint

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v2/order-book/estimate-buy-resource`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

## Headers

<table>
<thead>
<tr><th width="140">Name</th><th width="100">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key tied to your internal account. <a href="README.md">Get your API key</a>.</td></tr>
</tbody>
</table>

<mark style="color:red;">*</mark> Required.

## Request body

<table>
<thead>
<tr><th width="320">Field</th><th width="130">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>resourceAmount</code><mark style="color:red;">*</mark></td><td>Number</td><td>The number of resources.</td></tr>
<tr><td><code>unitPrice</code></td><td>String, Number</td><td>
<p><strong>"FAST", "MEDIUM", "SLOW", or number:</strong></p>
<p>- <strong>"FAST"</strong>: If the market is ready to fill = 100%, FAST = MEDIUM. If the market is ready to fill &#x3C; 100%, FAST = MEDIUM + 10. If market ready to fill = 0%, FAST = SLOW + 20.</p>
<p>- <strong>"MEDIUM"</strong>: The lowest price for the maximum market fill for this order. If market is ready to fill = 0%, MEDIUM = SLOW + 10.</p>
<p>- <strong>"SLOW"</strong>: The lowest price that can be set for this order.</p>
<p>- If the price is a number, the price unit is equal to SUN.</p>
</td></tr>
<tr><td><code>durationSec</code></td><td>Number</td><td>The duration of the bought resource, in seconds. Default 3d.</td></tr>
<tr><td><code>requester</code></td><td>String</td><td>The address of the requester.</td></tr>
<tr><td><code>receiver</code></td><td>String</td><td>The address of the resource receiver.</td></tr>
<tr><td><code>resourceType</code></td><td>String</td><td>"ENERGY" or "BANDWIDTH". Default: "ENERGY".</td></tr>
<tr><td><code>options</code></td><td>Object</td><td>Optional.</td></tr>
<tr><td><code>options.allowPartialFill</code></td><td>Boolean</td><td>Allow the order to be filled partially or not.</td></tr>
<tr><td><code>options.minResourceDelegateRequiredAmount</code></td><td>Number</td><td>The minimum resource amount delegated by a single provider.</td></tr>
</tbody>
</table>

<mark style="color:red;">*</mark> Required.

### Request body example

```json
{
    "resourceType": "ENERGY",
    "receiver": "TFwUFWr3QV376677Z8VWXxGUAMF123456",
    "durationSec": 259200,
    "resourceAmount": 32000,
    "unitPrice": "MEDIUM",
    "options": {
        "allowPartialFill": true,
        "minResourceDelegateRequiredAmount": 32000
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
        "unitPrice": 64,
        "durationSec": 259200,
        "estimateTrx": 6144000,
        "availableResource": 32000
    }
}
```

The `estimateTrx` value is returned in SUN (1 TRX = 1,000,000 SUN).

### Errors

{% tabs %}
{% tab title="401 API key required" %}
Returned when the `apikey` header is missing.

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```
{% endtab %}

{% tab title="401 Invalid API key" %}
Returned when the `apikey` header is present but not valid.

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```
{% endtab %}

{% tab title="400 Validation" %}
Returned when a required field is missing or invalid. The `message` names the offending field.

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'resourceAmount'"
}
```
{% endtab %}
{% endtabs %}

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.tronsave.io/v2/order-book/estimate-buy-resource" \
  -H "apikey: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "resourceType": "ENERGY",
    "receiver": "YOUR_TRON_ADDRESS",
    "durationSec": 259200,
    "resourceAmount": 32000,
    "unitPrice": "MEDIUM",
    "options": {
      "allowPartialFill": true,
      "minResourceDelegateRequiredAmount": 32000
    }
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const res = await fetch(
  "https://api.tronsave.io/v2/order-book/estimate-buy-resource",
  {
    method: "POST",
    headers: {
      "apikey": "YOUR_API_KEY",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      resourceType: "ENERGY",
      receiver: "YOUR_TRON_ADDRESS",
      durationSec: 259200,
      resourceAmount: 32000,
      unitPrice: "MEDIUM",
      options: {
        allowPartialFill: true,
        minResourceDelegateRequiredAmount: 32000,
      },
    }),
  }
);

const data = await res.json();
console.log(data);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v2/order-book/estimate-buy-resource"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
payload = {
    "resourceType": "ENERGY",
    "receiver": "YOUR_TRON_ADDRESS",
    "durationSec": 259200,
    "resourceAmount": 32000,
    "unitPrice": "MEDIUM",
    "options": {
        "allowPartialFill": True,
        "minResourceDelegateRequiredAmount": 32000,
    },
}

res = requests.post(url, headers=headers, json=payload)
print(res.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class EstimateTrx {
    public static void main(String[] args) throws Exception {
        String body = """
            {
              "resourceType": "ENERGY",
              "receiver": "YOUR_TRON_ADDRESS",
              "durationSec": 259200,
              "resourceAmount": 32000,
              "unitPrice": "MEDIUM",
              "options": {
                "allowPartialFill": true,
                "minResourceDelegateRequiredAmount": 32000
              }
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.tronsave.io/v2/order-book/estimate-buy-resource"))
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
	url := "https://api.tronsave.io/v2/order-book/estimate-buy-resource"
	payload := []byte(`{
		"resourceType": "ENERGY",
		"receiver": "YOUR_TRON_ADDRESS",
		"durationSec": 259200,
		"resourceAmount": 32000,
		"unitPrice": "MEDIUM",
		"options": {
			"allowPartialFill": true,
			"minResourceDelegateRequiredAmount": 32000
		}
	}`)

	req, _ := http.NewRequest("POST", url, bytes.NewBuffer(payload))
	req.Header.Set("apikey", "YOUR_API_KEY")
	req.Header.Set("Content-Type", "application/json")

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
use reqwest::blocking::Client;
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let payload = json!({
        "resourceType": "ENERGY",
        "receiver": "YOUR_TRON_ADDRESS",
        "durationSec": 259200,
        "resourceAmount": 32000,
        "unitPrice": "MEDIUM",
        "options": {
            "allowPartialFill": true,
            "minResourceDelegateRequiredAmount": 32000
        }
    });

    let res = client
        .post("https://api.tronsave.io/v2/order-book/estimate-buy-resource")
        .header("apikey", "YOUR_API_KEY")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()?;

    println!("{}", res.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* [Buy Energy (Create Order)](create-order.md) — place an order once you have an estimate.
* [Get Order Book](get-order-book.md) — inspect current pricing and availability.
* [Authentication](../../../authentication.md) — how to pass your API key on each request.
