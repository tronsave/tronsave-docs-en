---
description: Cancel one or more pending fast charge orders that have not yet been matched, using your API key.
---

# Cancel order

Cancel one or more fast charge orders by their order IDs. You can only cancel orders that are in **Pending** status and have not been matched yet.

To call this endpoint you need a TronSave API key tied to a prefunded internal account. See [Authentication](../../authentication.md) for how to obtain and use an API key.

## Endpoint

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/fast-charge-order-cancel`**

{% hint style="info" %}
**Rate limit:** 1 request per 2 seconds.
{% endhint %}

## Headers

<table><thead><tr><th width="131">Name</th><th width="135">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that is associated with the internal account. Required.</td></tr></tbody></table>

## Request body

<table><thead><tr><th width="190">Name</th><th width="164">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>order_ids</code></td><td>Array</td><td>Array of order IDs. A list of order IDs that have been created and need to be cancelled.</td></tr></tbody></table>

### Request body example

```json
{
    "order_ids": [
        "673c17a3129f1881382e98e3"
    ]
}
```

## Responses

### 200 OK — Success

```json
{
    "cancel_results": [
        {
            "order_id": "673c17a3129f1881382e98e3",
            "is_success": true,
            "fail_reason": null
        }
    ]
}
```

### 400 Bad Request — Validation error

Returned when the request body is missing a required field or has an invalid value. The `message` names the offending property.

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'order_ids'"
}
```

### 401 Unauthorized — Invalid API key

Returned when the `apikey` header is missing or invalid.

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY"
}
```

### 429 Too Many Requests — Rate limit

Returned when you exceed the rate limit (1 request per 2 seconds for this endpoint).

```json
{
    "error": true,
    "message": "Rate limit reached"
}
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST 'https://api.tronsave.io/v0/fast-charge-order-cancel' \
  -H 'apikey: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "order_ids": [
        "673c17a3129f1881382e98e3"
    ]
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const res = await fetch(
  "https://api.tronsave.io/v0/fast-charge-order-cancel",
  {
    method: "POST",
    headers: {
      apikey: "YOUR_API_KEY",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      order_ids: ["673c17a3129f1881382e98e3"],
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

url = "https://api.tronsave.io/v0/fast-charge-order-cancel"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "order_ids": ["673c17a3129f1881382e98e3"],
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

public class CancelOrder {
    public static void main(String[] args) throws Exception {
        String body = """
            {
                "order_ids": [
                    "673c17a3129f1881382e98e3"
                ]
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.tronsave.io/v0/fast-charge-order-cancel"))
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
		"order_ids": [
			"673c17a3129f1881382e98e3"
		]
	}`)

	url := "https://api.tronsave.io/v0/fast-charge-order-cancel"
	req, _ := http.NewRequest("POST", url, bytes.NewBuffer(body))
	req.Header.Set("apikey", "YOUR_API_KEY")
	req.Header.Set("Content-Type", "application/json")

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
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::blocking::Client::new();

    let body = json!({
        "order_ids": ["673c17a3129f1881382e98e3"]
    });

    let response = client
        .post("https://api.tronsave.io/v0/fast-charge-order-cancel")
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

## Next steps

* Review [Authentication](../../authentication.md) to set up your API key and internal account.
* See [Errors and rate limits](../../errors-and-rate-limits.md) for handling error responses.
* Learn about [Energy and Bandwidth](../../../concepts/energy-and-bandwidth.md).
