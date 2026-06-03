---
description: >-
  Create an Energy or Bandwidth purchase order with an API key, paid from your
  TronSave internal account, and receive an orderId to track its status.
---

# Create Order

Create an Energy or Bandwidth purchase order using an API key. The order is paid from your prefunded [internal account](../../../authentication.md), so no per-order on-chain signing is required. On success the endpoint returns an `orderId` you can use to track the order status.

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v2/buy-resource`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

## Headers

<table><thead><tr><th width="140">Name</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key tied to your internal account. See <a href="../../../authentication.md">Authentication</a> to get your API key.</td></tr></tbody></table>

<sub><mark style="color:red;">\*<mark style="color:red;"></sub> <sub></sub><sub>Required.</sub>

## Request body

<table><thead><tr><th width="239">Field</th><th width="140">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>resourceType</code></td><td>String</td><td><code>"ENERGY"</code> or <code>"BANDWIDTH"</code>. Default: <code>ENERGY</code>.</td></tr><tr><td><code>unitPrice</code></td><td>Number, String</td><td><p><code>"FAST"</code>, <code>"MEDIUM"</code>, <code>"SLOW"</code>, or a number. <strong>Default: <code>"MEDIUM"</code></strong>.</p><p><br>- <strong>FAST</strong>: If the market is ready to fill = 100%, FAST = MEDIUM. If the market is ready to fill &#x3C; 100%, FAST = MEDIUM + 10. If market ready to fill = 0%, FAST = SLOW + 20.</p><p>- <strong>MEDIUM</strong>: The lowest price for the maximum market fill for this order. If market is ready to fill = 0%, MEDIUM = SLOW + 10.</p><p>- <strong>SLOW</strong>: The lowest price that can be set for this order.</p><p>- If the price is a number, the price unit is SUN.</p></td></tr><tr><td><code>resourceAmount</code><mark style="color:red;">*</mark></td><td>Number</td><td>The number of resources.</td></tr><tr><td><code>receiver</code><mark style="color:red;">*</mark></td><td>String</td><td>Resource receiving address.</td></tr><tr><td><code>durationSec</code></td><td>Number</td><td>The duration of the bought resource, in seconds. <strong>Default:</strong> 259200 (3 days).</td></tr><tr><td><code>sponsor</code></td><td>String</td><td>Sponsor code.</td></tr><tr><td><code>options</code></td><td>Object</td><td>Optional.</td></tr><tr><td><code>options.allowPartialFill</code></td><td>Boolean</td><td>Allow the order to be filled partially or not.</td></tr><tr><td><code>options.onlyCreateWhenFulfilled</code></td><td>Boolean</td><td><p><code>true</code> => order only creates when it can be fulfilled.</p><p><code>false</code> => order will create even if it cannot be fulfilled.</p><p>Default value: <code>false</code>.</p></td></tr><tr><td><code>options.maxPriceAccepted</code></td><td>Number</td><td>Only create the order when the estimated price is less than this value.</td></tr><tr><td><code>options.preventDuplicateIncompleteOrders</code></td><td>Boolean</td><td><p><code>true</code> => only create if no <strong>uncompleted order</strong> with the same parameters exists.</p><p><code>false</code> => always create a new order, regardless of existing unfinished ones.</p><p>Default value: <strong><code>false</code></strong>.</p></td></tr><tr><td><code>options.minResourceDelegateRequiredAmount</code></td><td>Number</td><td>The minimum resource amount delegated by a single provider.</td></tr></tbody></table>

<sub><mark style="color:red;">\*<mark style="color:red;"></sub> <sub></sub><sub>Required.</sub>

### Request body example

```json
{
    "resourceType": "ENERGY",
    "receiver": "TFFbwz3UpmgaPT4UudwsxbiJf63t777777",
    "durationSec": 3600,
    "resourceAmount": 32000,
    "unitPrice": "MEDIUM",
    "options": {
        "allowPartialFill": true,
        "onlyCreateWhenFulfilled": true,
        "preventDuplicateIncompleteOrders": false,
        "maxPriceAccepted": 100,
        "minResourceDelegateRequiredAmount": 100000
    }
}
```

## Responses

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
    "MIN_PRICE_INVALID": "minPrice is less than the system's minimum price or not in the correct format.",
    "INTERNAL_ACCOUNT_NOT_FOUND": "internal account does not exist",
    "INTERNAL_BALANCE_ACCOUNT_TOO_LOW": "Balance is not enough",
    "CANNOT_FULFILLED": "The order requires an immediate full match, but the system cannot fulfill 100% of it.",
    "MUST_BE_WAIT_PREVIOUS_ORDER_FILLED": "A pending order with the same parameters already exists in the system.",
    "PRICE_EXCEED_MAX_PRICE_REQUIRED": "The price of the order exceeds the maximum price accepted."
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

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.tronsave.io/v2/buy-resource" \
  -H "apikey: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "resourceType": "ENERGY",
    "receiver": "YOUR_TRON_ADDRESS",
    "durationSec": 3600,
    "resourceAmount": 32000,
    "unitPrice": "MEDIUM",
    "options": {
      "allowPartialFill": true,
      "onlyCreateWhenFulfilled": false,
      "maxPriceAccepted": 100
    }
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const buyResource = async () => {
  const url = "https://api.tronsave.io/v2/buy-resource";
  const body = {
    resourceType: "ENERGY",
    unitPrice: "MEDIUM", // price in SUN, or "SLOW" | "MEDIUM" | "FAST"
    resourceAmount: 32000, // amount of resource to buy
    receiver: "YOUR_TRON_ADDRESS",
    durationSec: 3600, // order duration in seconds. Default: 259200 (3 days)
    options: {
      allowPartialFill: true,
      onlyCreateWhenFulfilled: false,
      maxPriceAccepted: 100,
    },
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
  //   "data": { "orderId": "6818426a65fa8ea36d119d2c" }
  // }
  return response;
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v2/buy-resource"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "resourceType": "ENERGY",
    "unitPrice": "MEDIUM",  # price in SUN, or "SLOW" | "MEDIUM" | "FAST"
    "resourceAmount": 32000,
    "receiver": "YOUR_TRON_ADDRESS",
    "durationSec": 3600,
    "options": {
        "allowPartialFill": True,
        "onlyCreateWhenFulfilled": False,
        "maxPriceAccepted": 100,
    },
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

public class CreateOrder {
    public static void main(String[] args) throws Exception {
        String url = "https://api.tronsave.io/v2/buy-resource";
        String body = """
            {
              "resourceType": "ENERGY",
              "unitPrice": "MEDIUM",
              "resourceAmount": 32000,
              "receiver": "YOUR_TRON_ADDRESS",
              "durationSec": 3600,
              "options": {
                "allowPartialFill": true,
                "onlyCreateWhenFulfilled": false,
                "maxPriceAccepted": 100
              }
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
	url := "https://api.tronsave.io/v2/buy-resource"
	body := []byte(`{
		"resourceType": "ENERGY",
		"unitPrice": "MEDIUM",
		"resourceAmount": 32000,
		"receiver": "YOUR_TRON_ADDRESS",
		"durationSec": 3600,
		"options": {
			"allowPartialFill": true,
			"onlyCreateWhenFulfilled": false,
			"maxPriceAccepted": 100
		}
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
    let url = "https://api.tronsave.io/v2/buy-resource";
    let body = json!({
        "resourceType": "ENERGY",
        "unitPrice": "MEDIUM",
        "resourceAmount": 32000,
        "receiver": "YOUR_TRON_ADDRESS",
        "durationSec": 3600,
        "options": {
            "allowPartialFill": true,
            "onlyCreateWhenFulfilled": false,
            "maxPriceAccepted": 100
        }
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

* Track an order with [Get Order Details](get-order-details.md).
* Preview cost first with [Estimate TRX](estimate-trx.md).
* Review the [Order Types](../../../../concepts/order-types.md) before placing orders.
