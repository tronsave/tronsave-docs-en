---
description: Estimate the TRX amount required to buy a given amount of Energy or Bandwidth for a chosen rental duration.
---

# Estimate TRX

Estimate the required TRX amount for purchasing Energy or Bandwidth resources before placing an order. The endpoint returns the resolved unit price, the duration, the estimated TRX cost, and how much of the requested resource is currently available to fill.

## Endpoint

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v2/order-book/estimate-buy-resource`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

## Headers

<table><thead><tr><th width="106">Name</th><th width="94">Type</th><th>Description</th></tr></thead><tbody><tr><td>apikey<mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that belongs to your internal account. See <a href="../../authentication.md">Authentication</a>.</td></tr></tbody></table>

## Request body

<table><thead><tr><th width="208.33333333333331">Field</th><th width="95">Type</th><th>Description</th></tr></thead><tbody><tr><td>resourceAmount<mark style="color:red;">*</mark></td><td>Number</td><td>The number of resources.</td></tr><tr><td>unitPrice</td><td>String, Number</td><td><p><strong>"FAST", "MEDIUM", "SLOW", or number</strong>:</p><p><br>-"FAST": If the market is ready to fill = 100%, FAST = MEDIUM. If the market is ready to fill &#x3C; 100%, FAST = MEDIUM + 10. If market ready to fill = 0%, FAST = SLOW + 20.</p><p></p><p>-"MEDIUM": The lowest price for the maximum market fill for this order. If market is ready to fill = 0%, MEDIUM = SLOW + 10.</p><p></p><p>-"SLOW": The lowest price that can be set for this order.</p><p></p><p>-If the price is a number, the price unit is equal to SUN</p></td></tr><tr><td>durationSec</td><td>Number</td><td>The duration of the bought resource, time unit, is in seconds. Default 3d</td></tr><tr><td>requester</td><td>String</td><td>The address of requester.</td></tr><tr><td>receiver</td><td>String</td><td>The address of receiver resource.</td></tr><tr><td>resourceType</td><td>String</td><td>"ENERGY" or "BANDWIDTH", default: "ENERGY"</td></tr><tr><td>options</td><td>Object</td><td>optional</td></tr><tr><td>options.allowPartialFill</td><td>Boolean</td><td>Allow the order to be filled partially or not</td></tr><tr><td>options.minResourceDelegateRequiredAmount</td><td>Number</td><td>The minimum resource amount delegated by a single provider.</td></tr></tbody></table>

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

**`401 Unauthorized`** — the `apikey` header is missing.

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```

**`401 Unauthorized`** — the supplied API key is invalid.

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```

**`400 Bad Request`** — a required field is missing or invalid. The `message` names the offending field.

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'resourceAmount'"
}
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST 'https://api.tronsave.io/v2/order-book/estimate-buy-resource' \
  -H 'apikey: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
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
      apikey: "YOUR_API_KEY",
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
body = {
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

res = requests.post(url, headers=headers, json=body)
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
	body := []byte(`{
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

	req, _ := http.NewRequest(
		"POST",
		"https://api.tronsave.io/v2/order-book/estimate-buy-resource",
		bytes.NewBuffer(body),
	)
	req.Header.Set("apikey", "YOUR_API_KEY")
	req.Header.Set("Content-Type", "application/json")

	res, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer res.Body.Close()

	out, _ := io.ReadAll(res.Body)
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let body = json!({
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

    let client = reqwest::blocking::Client::new();
    let res = client
        .post("https://api.tronsave.io/v2/order-book/estimate-buy-resource")
        .header("apikey", "YOUR_API_KEY")
        .header("Content-Type", "application/json")
        .json(&body)
        .send()?;

    println!("{}", res.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* Place the order once you are happy with the estimate: [Buy Energy (Create Order)](api-key/create-order.md).
* Review available pricing tiers in the [Get Order Book](api-key/get-order-book.md) endpoint.
* Learn the difference between [Energy and Bandwidth](../../../concepts/energy-and-bandwidth.md).
