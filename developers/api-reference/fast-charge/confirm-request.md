---
description: Confirm completed fast charge orders to end the Energy rental, reclaim unused Energy, and trigger the TRX refund to your internal account.
---

# Confirm Request

After you have used the Energy, confirm the orders you want to complete so the system ends the rental. Confirming reclaims any unused Energy and calculates the TRX refund returned to your internal account. The sooner you confirm, the more cost you save.

To call this endpoint you need a TronSave API key tied to a prefunded internal account. See [Authentication](../../authentication.md) for how to obtain and use an API key.

## Endpoint

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/fast-charge-order-confirmation`**

{% hint style="info" %}
**Rate limit:** 1 request per 2 seconds.
{% endhint %}

{% hint style="success" %}
Confirming early helps you receive the **maximum possible refund**.
{% endhint %}

## Headers

<table><thead><tr><th width="131">Name</th><th width="135">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that is associated with the internal account. Required.</td></tr></tbody></table>

## Request body

<table><thead><tr><th width="190">Name</th><th width="164">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>order_ids</code></td><td>Array</td><td>A list of order IDs that have been created and need to <strong>be confirmed</strong>.</td></tr></tbody></table>

### Request body example

```json
{
    "order_ids": [
        "673d5fe2d2451e67c4d09483"
    ]
}
```

## Responses

### 200 OK — Success

```json
{
    "confirmation_results": [
        {
            "order_id": string,    // id of order
            "is_success": boolean, // the status of the confirm action
            "fail_reason": string  // By default, a successful confirm returns null. If the confirm fails, it returns the reason for failure.
        }
    ]
}
```

Example success response:

```json
{
    "confirmation_results": [
        {
            "order_id": "673d5fe2d2451e67c4d09483",
            "is_success": true,
            "fail_reason": null
        }
    ]
}
```

### Errors

This is a legacy `v0` endpoint authenticated with an `apikey` header.

#### 401 Unauthorized — missing API key

Returned when the `apikey` header is absent.

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED"
}
```

#### 401 Unauthorized — invalid API key

Returned when the supplied `apikey` is not valid.

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY"
}
```

#### 400 Bad Request — validation error

Returned when a required field (such as `order_ids`) is missing or invalid. The message names the offending property.

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'order_ids'"
}
```

#### 429 Too Many Requests — rate limit

Returned when you exceed the rate limit for this endpoint (1 request per 2 seconds).

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
curl -X POST 'https://api.tronsave.io/v0/fast-charge-order-confirmation' \
  -H 'apikey: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "order_ids": [
        "673d5fe2d2451e67c4d09483"
    ]
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const res = await fetch(
  "https://api.tronsave.io/v0/fast-charge-order-confirmation",
  {
    method: "POST",
    headers: {
      apikey: "YOUR_API_KEY",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      order_ids: ["673d5fe2d2451e67c4d09483"],
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

url = "https://api.tronsave.io/v0/fast-charge-order-confirmation"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "order_ids": ["673d5fe2d2451e67c4d09483"],
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

public class ConfirmRequest {
    public static void main(String[] args) throws Exception {
        String body = """
            {
                "order_ids": [
                    "673d5fe2d2451e67c4d09483"
                ]
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.tronsave.io/v0/fast-charge-order-confirmation"))
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
			"673d5fe2d2451e67c4d09483"
		]
	}`)

	url := "https://api.tronsave.io/v0/fast-charge-order-confirmation"
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
        "order_ids": ["673d5fe2d2451e67c4d09483"]
    });

    let response = client
        .post("https://api.tronsave.io/v0/fast-charge-order-confirmation")
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

* Cancel a still-pending order with [Cancel Order](cancel-order.md).
* Review your completed rents with [Get History](get-history.md).
* See [Authentication](../../authentication.md) to set up your API key and internal account.
* See [Errors and rate limits](../../errors-and-rate-limits.md) for handling error responses.
