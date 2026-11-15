---
description: Create one or more Fast Charge orders to rent Energy for a list of receiver addresses in a single request, authenticated with your TronSave API key.
---

# Create Fast Charge Orders

Submit a Fast Charge order with a maximum acceptable price, a rental duration, and the list of addresses that should receive Energy. The endpoint returns the IDs of the created orders so you can track and later confirm them.

{% hint style="info" %}
Fast Charge is an API-key feature. Every call is authenticated against a prefunded TronSave internal account. See [Authentication](../../authentication.md) for how to obtain a key and fund your account.
{% endhint %}

## Endpoint

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/fast-charge-order-request`**

{% hint style="info" %}
**Rate limit:** 1 request per 2 seconds.
{% endhint %}

## Headers

<table><thead><tr><th width="131">Name</th><th width="135">Type</th><th>Description</th></tr></thead><tbody><tr><td>apikey<mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that belongs to your internal account. See <a href="../../authentication.md">Authentication</a>.</td></tr></tbody></table>

## Request body

<table><thead><tr><th width="190">Name</th><th width="137">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>amount</code></td><td>Number</td><td>Amount of resource want to buy.</td></tr><tr><td><code>duration_sec</code></td><td>Number</td><td>The duration of the rent resource, the time unit is equal second.</td></tr><tr><td><code>max_price_accept</code></td><td>Number</td><td>Only create an order when the estimated price is less than this value.</td></tr><tr><td><code>receivers</code></td><td>Array</td><td>Array receiver data. A list of wallet addresses receiving Energy.</td></tr><tr><td><code>deadline</code></td><td>Number</td><td>Maximum matching time in <strong>seconds</strong>. Exceeding it cancels the order and refunds.</td></tr></tbody></table>

### Request body example

```json
{
    "max_price_accept": 70,
    "amount": 130000,
    "duration_sec": 900,
    "deadline": 300,
    "receivers": [
        "TQk2eKHE9ZfCdVmyPnh8DfRMUF0123456",
        "TQk2eKHE9ZfCdVmyPnh8DfRMUF1111111"
    ]
}
```

## Response

### Success

Returns an array of order IDs.

```json
{
    "order_ids": [
        "673c17a3129f1881382e98e2",
        "673c17a3129f1881382e98e3"
    ]
}
```

### Error

**`401 Unauthorized`** — the `apikey` header is missing:

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED"
}
```

**`401 Unauthorized`** — the supplied API key is invalid:

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY"
}
```

**`400 Bad Request`** — a required field is missing or invalid. The message names the offending field:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'receivers'"
}
```

The endpoint may also return business-logic `400` errors (for example when the prefunded internal account balance is too low to cover the order). <!-- [NEEDS CONFIRMATION: exact business-logic 400 message bodies for insufficient balance / unfulfillable orders on this endpoint] -->

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.tronsave.io/v0/fast-charge-order-request" \
  -H "apikey: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "max_price_accept": 70,
    "amount": 130000,
    "duration_sec": 900,
    "deadline": 300,
    "receivers": [
        "YOUR_TRON_ADDRESS",
        "YOUR_TRON_ADDRESS"
    ]
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const res = await fetch(
  "https://api.tronsave.io/v0/fast-charge-order-request",
  {
    method: "POST",
    headers: {
      apikey: "YOUR_API_KEY",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      max_price_accept: 70,
      amount: 130000,
      duration_sec: 900,
      deadline: 300,
      receivers: ["YOUR_TRON_ADDRESS", "YOUR_TRON_ADDRESS"],
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

url = "https://api.tronsave.io/v0/fast-charge-order-request"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "max_price_accept": 70,
    "amount": 130000,
    "duration_sec": 900,
    "deadline": 300,
    "receivers": ["YOUR_TRON_ADDRESS", "YOUR_TRON_ADDRESS"],
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

public class CreateFastChargeOrder {
    public static void main(String[] args) throws Exception {
        String body = """
            {
                "max_price_accept": 70,
                "amount": 130000,
                "duration_sec": 900,
                "deadline": 300,
                "receivers": [
                    "YOUR_TRON_ADDRESS",
                    "YOUR_TRON_ADDRESS"
                ]
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.tronsave.io/v0/fast-charge-order-request"))
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
		"max_price_accept": 70,
		"amount": 130000,
		"duration_sec": 900,
		"deadline": 300,
		"receivers": [
			"YOUR_TRON_ADDRESS",
			"YOUR_TRON_ADDRESS"
		]
	}`)

	req, _ := http.NewRequest(
		"POST",
		"https://api.tronsave.io/v0/fast-charge-order-request",
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
        "max_price_accept": 70,
        "amount": 130000,
        "duration_sec": 900,
        "deadline": 300,
        "receivers": ["YOUR_TRON_ADDRESS", "YOUR_TRON_ADDRESS"]
    });

    let client = reqwest::blocking::Client::new();
    let res = client
        .post("https://api.tronsave.io/v0/fast-charge-order-request")
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

* Track each order's matching status: [Tracking Fast Charge Order](track-order.md).
* Confirm completed orders to reclaim unused Energy and refund the difference: [Confirm Request](confirm-request.md).
* Need a key first? See [Authentication](../../authentication.md).
