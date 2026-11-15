---
description: Retrieve your Fast Charge order history, sorted by creation time with optional time-range and pagination filters.
---

# Get Fast Charge History

Fetch your past Fast Charge orders, sorted by creation time (newest first). By default the endpoint returns the 10 most recent orders; use the query parameters to filter by time range and paginate.

{% hint style="info" %}
This endpoint requires an API key. See [Authentication](../../authentication.md) for how to obtain a key and fund your internal account.
{% endhint %}

## Endpoint

<mark style="color:blue;">`GET`</mark> `https://api.tronsave.io/v0/fast-charge-order-history`

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

## Query parameters

<table>
  <thead>
    <tr><th width="131">Name</th><th width="135">Type</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr><td><code>from</code></td><td>String</td><td>Start time in milliseconds.</td></tr>
    <tr><td><code>to</code></td><td>String</td><td>End time in milliseconds.</td></tr>
    <tr><td><code>page</code></td><td>Integer</td><td>Page index, starting from 0. Default: 0.</td></tr>
    <tr><td><code>pageSize</code></td><td>Integer</td><td>Number of orders per page. Default: 10.</td></tr>
  </tbody>
</table>

### Example request URL

<mark style="color:blue;">`GET`</mark> `https://api.tronsave.io/v0/fast-charge-order-history?from=1732066773000&to=1732073973000&page=0&pageSize=10`

## Response

### Success

The response contains the total number of matching orders and an array of order objects.

```json
{
    "total": "number",
    "results": [
        {
            "_id": "string",                       // id of order
            "receiver": "string",                  // the address that received the resource
            "resource_type": "string",             // the resource type, e.g. "ENERGY"
            "amount": "number",                    // the amount of resource
            "options": {
                "deadline": "number",              // Maximum matching time in seconds. Exceeding it cancels the order and refunds.
                "max_price_accept": "number"       // Only create an order when the estimated price is less than this value.
            },
            "charge_amount": "number",             // Total payout estimated for this order
            "status": "string",                    // status of this order
            "requester": "string",                 // the address that owns the order
            "unit_price": "number",                // price unit, expressed in SUN
            "is_matched": "boolean",               // matched status of this order
            "charge_duration_sec": "number",       // rent duration, in seconds
            "delegate_amount_in_sun": "number",    // The amount of resource delegated
            "delegate_txid": "string",             // on-chain delegate transaction
            "delegator": "string",                 // The address that delegated resource to the target address
            "paid_amount_actual": "number",        // Actual amount of TRX paid for this order
            "repay_amount": "number"               // The refunded amount, added directly to the internal account
            // ...
        }
    ]
}
```

### Success (example)

```json
{
    "total": 1,
    "results": [
        {
            "_id": "673d586ad2451e67c4d0943b",
            "receiver": "TQk2eKHE9ZfCdVmyPnh8DfRMUF5b123456",
            "resource_type": "ENERGY",
            "amount": 65000,
            "options": {
                "deadline": 300,
                "max_price_accept": 50
            },
            "charge_amount": 4485000,
            "status": "Completed",
            "pay_txid": "internal_fast_charge_673d586ad2451e67c4d0943a",
            "request_id": "673d586ad2451e67c4d0943a",
            "requester": "TLoE6dE6EaEfeH6pbWHrtcTfMLtk111111",
            "unit_price": 69,
            "is_matched": true,
            "charge_duration_sec": 900,
            "scan_times": 1,
            "created_at": "2024-11-20T03:32:58.722Z",
            "update_at": "2024-11-20T03:34:43.815Z",
            "delegate_amount_in_sun": 881000000,
            "delegate_at": "2024-11-20T03:33:37.538Z",
            "delegate_txid": "ed287d774bbe2e673fc938feb9a02bc926bc82198ef02292212132aec4951821",
            "delegator": "TQBV7xU489Rq8ZCsYi72zBhJMdr2222222",
            "expire_delegate_at": "2024-11-20T03:38:37.530Z",
            "matched_resource_order_id": "673d58d3d2451e67c4d09440",
            "paid_amount_actual": 4127500,
            "repay_amount": 357500,
            "repay_at": "2024-11-20T03:34:43.815Z",
            "repay_by": "Confirmed",
            "repay_txid": "673d58d3d2451e67c4d09441",
            "resource_order_request_id": "673d58d3d2451e67c4d0943f"
        }
    ]
}
```

### Errors

This endpoint authenticates with the `apikey` header. Authentication and validation failures return the following bodies.

**401 Unauthorized** — the `apikey` header is missing:

```json
{ "error": true, "message": "TSAS:106 API_KEY_REQUIRED" }
```

**401 Unauthorized** — the supplied API key is invalid:

```json
{ "error": true, "message": "TSAS:107 INVALID_API_KEY" }
```

**400 Bad Request** — a query parameter fails schema validation (the message names the offending field):

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "querystring/pageSize must be integer"
}
```

**429 Too Many Requests** — the rate limit (1 request per 2 seconds for this endpoint) is exceeded:

```json
{ "error": true, "message": "Rate limit reached" }
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.tronsave.io/v0/fast-charge-order-history?from=1732066773000&to=1732073973000&page=0&pageSize=10" \
  -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const params = new URLSearchParams({
  from: "1732066773000",
  to: "1732073973000",
  page: "0",
  pageSize: "10",
});

const res = await fetch(
  `https://api.tronsave.io/v0/fast-charge-order-history?${params}`,
  {
    method: "GET",
    headers: {
      apikey: "YOUR_API_KEY",
    },
  }
);

const data = await res.json();
console.log(data);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v0/fast-charge-order-history"
headers = {
    "apikey": "YOUR_API_KEY",
}
params = {
    "from": "1732066773000",
    "to": "1732073973000",
    "page": 0,
    "pageSize": 10,
}

res = requests.get(url, headers=headers, params=params)
print(res.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetHistory {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();

        String url = "https://api.tronsave.io/v0/fast-charge-order-history"
            + "?from=1732066773000&to=1732073973000&page=0&pageSize=10";

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create(url))
            .header("apikey", "YOUR_API_KEY")
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
	url := "https://api.tronsave.io/v0/fast-charge-order-history" +
		"?from=1732066773000&to=1732073973000&page=0&pageSize=10"

	req, _ := http.NewRequest("GET", url, nil)
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

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let res = client
        .get("https://api.tronsave.io/v0/fast-charge-order-history")
        .query(&[
            ("from", "1732066773000"),
            ("to", "1732073973000"),
            ("page", "0"),
            ("pageSize", "10"),
        ])
        .header("apikey", "YOUR_API_KEY")
        .send()?;

    println!("{}", res.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* [Tracking Fast Charge Order](track-order.md) — check the matching status of specific orders by ID.
* [Fast Charge overview](README.md) · [Authentication](../../authentication.md) · [Errors & Rate Limits](../../errors-and-rate-limits.md)
