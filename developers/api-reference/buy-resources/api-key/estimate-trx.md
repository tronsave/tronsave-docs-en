---
description: >-
  Estimate the TRX required to buy a given amount of Energy or Bandwidth for a
  chosen rental duration, using your TronSave API key.
---

# Estimate TRX

Use this endpoint to calculate how much TRX an order will cost before you place it. Pass the resource amount, rental duration, and a unit price (or a speed tier), and TronSave returns the resolved unit price, the estimated TRX, and how much of the requested resource is currently available.

## Endpoint

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v2/estimate-buy-resource`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

## Headers

| Header         | Value              | Required |
| -------------- | ------------------ | -------- |
| `Content-Type` | `application/json` | Yes      |

## Request body

<table><thead><tr><th width="320">Field</th><th width="130">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>resourceAmount</code><mark style="color:red;">*</mark></td><td>Number</td><td>The number of resources.</td></tr><tr><td><code>unitPrice</code></td><td>String, Number</td><td><p><strong>"FAST", "MEDIUM", "SLOW", or number:</strong></p><p>- <strong>"FAST"</strong>: If the market is ready to fill = 100%, FAST = MEDIUM. If the market is ready to fill &#x3C; 100%, FAST = MEDIUM + 10. If market ready to fill = 0%, FAST = SLOW + 20.</p><p>- <strong>"MEDIUM"</strong>: The lowest price for the maximum market fill for this order. If market is ready to fill = 0%, MEDIUM = SLOW + 10.</p><p>- <strong>"SLOW"</strong>: The lowest price that can be set for this order.</p><p>- If the price is a number, the price unit is equal to SUN.</p></td></tr><tr><td><code>durationSec</code></td><td>Number</td><td>The duration of the bought resource, in seconds. Default 3d.</td></tr><tr><td><code>requester</code></td><td>String</td><td>The address of the requester.</td></tr><tr><td><code>receiver</code></td><td>String</td><td>The address of the resource receiver.</td></tr><tr><td><code>resourceType</code></td><td>String</td><td>"ENERGY" or "BANDWIDTH". Default: "ENERGY".</td></tr><tr><td><code>options</code></td><td>Object</td><td>Optional.</td></tr><tr><td><code>options.allowPartialFill</code></td><td>Boolean</td><td>Allow the order to be filled partially or not.</td></tr><tr><td><code>options.minResourceDelegateRequiredAmount</code></td><td>Number</td><td>The minimum resource amount delegated by a single provider.</td></tr></tbody></table>

<mark style="color:red;">\*</mark> Required.

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

{% tab title="404 Not Found" %}
404 Not Found — wrong route/path:

```json
{
    "message": "Route POST:/v2/... not found",
    "error": "Not Found",
    "statusCode": 404
}
```
{% endtab %}
{% endtabs %}

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://api.tronsave.io/v2/estimate-buy-resource \
  -H "Content-Type: application/json" \
  -d '{
    "resourceType": "ENERGY",
    "receiver": "YOUR_TRON_ADDRESS",
    "durationSec": 259200,
    "resourceAmount": 32000,
    "unitPrice": "MEDIUM",
    "options": {
      "allowPartialFill": true
    }
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API_URL = "https://api.tronsave.io";

const getEstimate = async (requesterAddress, receiverAddress, resourceAmount, durationSec) => {
  const url = `${TRONSAVE_API_URL}/v2/estimate-buy-resource`;
  const body = {
    resourceAmount,
    unitPrice: "MEDIUM",
    resourceType: "ENERGY",
    durationSec,
    requester: requesterAddress,
    receiver: receiverAddress,
    options: {
      allowPartialFill: true,
    },
  };

  const res = await fetch(url, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(body),
  });

  return res.json();
};

// getEstimate("YOUR_TRON_ADDRESS", "YOUR_TRON_ADDRESS", 32000, 259200);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"


def get_estimate(requester, receiver, resource_amount, duration_sec):
    url = f"{TRONSAVE_API_URL}/v2/estimate-buy-resource"
    body = {
        "resourceAmount": resource_amount,
        "unitPrice": "MEDIUM",
        "resourceType": "ENERGY",
        "durationSec": duration_sec,
        "requester": requester,
        "receiver": receiver,
        "options": {
            "allowPartialFill": True,
        },
    }

    response = requests.post(url, json=body)
    return response.json()


# get_estimate("YOUR_TRON_ADDRESS", "YOUR_TRON_ADDRESS", 32000, 259200)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class EstimateTrx {
    static final String TRONSAVE_API_URL = "https://api.tronsave.io";

    public static void main(String[] args) throws Exception {
        String body = """
            {
              "resourceType": "ENERGY",
              "receiver": "YOUR_TRON_ADDRESS",
              "durationSec": 259200,
              "resourceAmount": 32000,
              "unitPrice": "MEDIUM",
              "options": {
                "allowPartialFill": true
              }
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(TRONSAVE_API_URL + "/v2/estimate-buy-resource"))
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
	body := []byte(`{
		"resourceType": "ENERGY",
		"receiver": "YOUR_TRON_ADDRESS",
		"durationSec": 259200,
		"resourceAmount": 32000,
		"unitPrice": "MEDIUM",
		"options": {
			"allowPartialFill": true
		}
	}`)

	req, err := http.NewRequest("POST", tronsaveAPIURL+"/v2/estimate-buy-resource", bytes.NewBuffer(body))
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
use reqwest::blocking::Client;
use serde_json::json;

const TRONSAVE_API_URL: &str = "https://api.tronsave.io";

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let body = json!({
        "resourceType": "ENERGY",
        "receiver": "YOUR_TRON_ADDRESS",
        "durationSec": 259200,
        "resourceAmount": 32000,
        "unitPrice": "MEDIUM",
        "options": {
            "allowPartialFill": true
        }
    });

    let client = Client::new();
    let response = client
        .post(format!("{}/v2/estimate-buy-resource", TRONSAVE_API_URL))
        .header("Content-Type", "application/json")
        .json(&body)
        .send()?;

    println!("{}", response.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* [Buy Energy (Create Order)](create-order.md) — place an order once you have an estimate.
* [Get Order Book](get-order-book.md) — inspect current pricing and availability.
* [Authentication](../../../authentication.md) — how to pass your API key on each request.
