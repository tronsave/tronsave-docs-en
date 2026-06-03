---
description: Check the matching status of one or more Fast Charge orders by their order IDs.
---

# Tracking Fast Charge Order

After creating Fast Charge orders, use this endpoint to check their status — whether each order is still waiting to be matched or has already been matched.

{% hint style="info" %}
This endpoint requires an API key. See [Authentication](../../authentication.md) for how to obtain a key and fund your internal account.
{% endhint %}

## Endpoint

<mark style="color:orange;">`POST`</mark> `https://api.tronsave.io/v0/fast-charge-order-tracking`

**Rate limit:** 1 request per 2 seconds.

## Headers

<table>
  <thead>
    <tr><th width="131">Name</th><th width="135">Type</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key associated with your internal account.</td></tr>
  </tbody>
</table>

## Request body

<table>
  <thead>
    <tr><th width="190">Name</th><th width="164">Type</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr><td><code>order_ids</code></td><td>Array</td><td>Array of order IDs. A list of order IDs that have been created and need their status checked.</td></tr>
  </tbody>
</table>

### Request body example

```json
{
    "order_ids": [
        "673c17a3129f1881382e98e2",
        "673c17a3129f1881382e98e3"
    ]
}
```

## Response

### Success

```json
{
    "results": [
        {
            "order_id": "673c17a3129f1881382e98e2",
            "status": "Completed"
        },
        {
            "order_id": "673c17a3129f1881382e98e3",
            "status": "Pending"
        }
    ]
}
```

### Errors

This endpoint authenticates with the `apikey` header.

**401 Unauthorized** — the `apikey` header is missing:

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED"
}
```

**401 Unauthorized** — the API key is invalid:

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY"
}
```

**400 Bad Request** — a required field is missing or invalid. The message names the offending field:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'order_ids'"
}
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.tronsave.io/v0/fast-charge-order-tracking" \
  -H "Content-Type: application/json" \
  -H "apikey: YOUR_API_KEY" \
  -d '{
    "order_ids": [
        "673c17a3129f1881382e98e2",
        "673c17a3129f1881382e98e3"
    ]
}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const res = await fetch("https://api.tronsave.io/v0/fast-charge-order-tracking", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    apikey: "YOUR_API_KEY",
  },
  body: JSON.stringify({
    order_ids: [
      "673c17a3129f1881382e98e2",
      "673c17a3129f1881382e98e3",
    ],
  }),
});

const data = await res.json();
console.log(data);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v0/fast-charge-order-tracking"
headers = {
    "Content-Type": "application/json",
    "apikey": "YOUR_API_KEY",
}
body = {
    "order_ids": [
        "673c17a3129f1881382e98e2",
        "673c17a3129f1881382e98e3",
    ],
}

res = requests.post(url, json=body, headers=headers)
print(res.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class TrackOrder {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();

        String body = """
            {
                "order_ids": [
                    "673c17a3129f1881382e98e2",
                    "673c17a3129f1881382e98e3"
                ]
            }
            """;

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.tronsave.io/v0/fast-charge-order-tracking"))
            .header("Content-Type", "application/json")
            .header("apikey", "YOUR_API_KEY")
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
		"order_ids": [
			"673c17a3129f1881382e98e2",
			"673c17a3129f1881382e98e3"
		]
	}`)

	req, _ := http.NewRequest(
		"POST",
		"https://api.tronsave.io/v0/fast-charge-order-tracking",
		bytes.NewBuffer(body),
	)
	req.Header.Set("Content-Type", "application/json")
	req.Header.Set("apikey", "YOUR_API_KEY")

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
use reqwest::blocking::Client;
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let body = json!({
        "order_ids": [
            "673c17a3129f1881382e98e2",
            "673c17a3129f1881382e98e3"
        ]
    });

    let res = client
        .post("https://api.tronsave.io/v0/fast-charge-order-tracking")
        .header("Content-Type", "application/json")
        .header("apikey", "YOUR_API_KEY")
        .json(&body)
        .send()?;

    println!("{}", res.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* [Confirm Request](confirm-request.md) — complete matched orders to reclaim unused Energy and receive a TRX refund.
* [Cancel Order](cancel-order.md) — cancel a `Pending` order for a full refund.
* [Fast Charge overview](README.md) · [Authentication](../../authentication.md) · [Errors & Rate Limits](../../errors-and-rate-limits.md)
