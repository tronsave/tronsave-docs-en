---
description: Estimate the TRX required to create fast charge orders before placing them, using your API key and prefunded internal account.
---

# Estimate TRX

Estimate the total TRX needed to create one or more fast charge orders before placing them. The endpoint resolves the unit price, reports how many orders the system can currently match, and returns your current internal account balance.

To call this endpoint you need a TronSave API key tied to a prefunded internal account. See [Authentication](../../authentication.md) for how to obtain and use an API key.

## Endpoint

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/fast-charge-estimate-order-request`**

{% hint style="info" %}
**Rate limit:** 1 request per 2 seconds.
{% endhint %}

## Headers

<table><thead><tr><th width="131">Name</th><th width="135">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that is associated with the internal account. Required.</td></tr></tbody></table>

## Request body

<table><thead><tr><th width="190">Name</th><th width="164">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>receiver_count</code></td><td>String</td><td>Number of orders to create.</td></tr><tr><td><code>amount</code></td><td>Number</td><td>Amount of resource you want to buy.</td></tr><tr><td><code>max_price_accept</code></td><td>Number</td><td>Only create an order when the estimated price is less than this value.</td></tr><tr><td><code>resource_type</code></td><td>String</td><td><code>"ENERGY"</code></td></tr><tr><td><code>duration_sec</code></td><td>Number</td><td>The duration of the resource rental, in seconds.</td></tr></tbody></table>

### Request body example

```json
{
    "receiver_count": 10,
    "amount": 130000,
    "max_price_accept": 70,
    "resource_type": "ENERGY",
    "duration_sec": 900
}
```

## Responses

### 200 OK — Success

```json
{
    "charge_amount": number,                // Total payout for all orders
    "price": number,                        // Price, unit is SUN
    "max_receiver_can_provide": number,     // The maximum number of orders the system is ready to match
    "internal_balance": string,             // Balance of your internal account
    "duration_sec": number                  // Rent duration, unit is seconds
}
```

Example success response:

```json
{
    "charge_amount": 44850000,
    "price": 69,
    "max_receiver_can_provide": 8,
    "able_to_match": true,
    "internal_balance": "117505470"
}
```

### 400 Bad Request — Validation error

Returned when a required field is missing or invalid. The message names the offending field:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "body must have required property 'receiver_count'"
}
```

The endpoint may also return these business-logic errors:

```json
{
    "INTERNAL_ACCOUNT_NOT_FOUND": "internal account not exists",
    "INTERNAL_BALANCE_ACCOUNT_TOO_LOW": "Balance is not enough"
}
```

### 401 Unauthorized — Missing or invalid API key

Returned when the `apikey` header is missing:

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED"
}
```

Returned when the `apikey` header is present but invalid:

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY"
}
```

### 429 Too Many Requests — Rate limit reached

```json
{
    "RATE_LIMIT": "Rate limit reached"
}
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST 'https://api.tronsave.io/v0/fast-charge-estimate-order-request' \
  -H 'apikey: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "receiver_count": 10,
    "amount": 130000,
    "max_price_accept": 70,
    "resource_type": "ENERGY",
    "duration_sec": 900
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const res = await fetch(
  "https://api.tronsave.io/v0/fast-charge-estimate-order-request",
  {
    method: "POST",
    headers: {
      apikey: "YOUR_API_KEY",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      receiver_count: 10,
      amount: 130000,
      max_price_accept: 70,
      resource_type: "ENERGY",
      duration_sec: 900,
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

url = "https://api.tronsave.io/v0/fast-charge-estimate-order-request"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "receiver_count": 10,
    "amount": 130000,
    "max_price_accept": 70,
    "resource_type": "ENERGY",
    "duration_sec": 900,
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

public class EstimateTrx {
    public static void main(String[] args) throws Exception {
        String body = """
            {
                "receiver_count": 10,
                "amount": 130000,
                "max_price_accept": 70,
                "resource_type": "ENERGY",
                "duration_sec": 900
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.tronsave.io/v0/fast-charge-estimate-order-request"))
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
		"receiver_count": 10,
		"amount": 130000,
		"max_price_accept": 70,
		"resource_type": "ENERGY",
		"duration_sec": 900
	}`)

	url := "https://api.tronsave.io/v0/fast-charge-estimate-order-request"
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
        "receiver_count": 10,
        "amount": 130000,
        "max_price_accept": 70,
        "resource_type": "ENERGY",
        "duration_sec": 900
    });

    let response = client
        .post("https://api.tronsave.io/v0/fast-charge-estimate-order-request")
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
* See [Errors and rate limits](../../errors-and-rate-limits.md) for handling the error responses above.
* Learn about [Energy and Bandwidth](../../../concepts/energy-and-bandwidth.md).
