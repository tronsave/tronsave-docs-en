---
description: >-
  Retrieve a buyer account's monthly trading statistics for the last 6 months.
  Requires a whitelisted API key.
---

# Monthly Stats

Retrieve detailed monthly statistics and trading activity for the buyer account. This endpoint returns per-month order counts and TRX deposit totals for the most recent 6 months.

{% hint style="warning" %}
**Whitelist required**\
This endpoint is only available to whitelisted addresses. Log in to TronSave and send your wallet address to the support team so it can be added to the whitelist. Once whitelisted, use the API key generated from that wallet address to call this endpoint.

Contact support via Telegram: [**@wantingtrx**](https://t.me/wantingtrx)
{% endhint %}

## Endpoint

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v2/internal/buyer/monthly-stats`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

## Headers

<table><thead><tr><th width="120">Name</th><th width="100">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key. Must belong to a whitelisted address. See <a href="../../../authentication.md">Authentication</a> to get your API key.</td></tr></tbody></table>

<mark style="color:red;">\*</mark> Required.

## Request body

This endpoint takes no request body — pass the `apikey` header only.

## Response

A successful response returns `error: false` and the statistics window in `data`.

<table><thead><tr><th width="280">Field</th><th width="100">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>data.fromMonth</code></td><td>String</td><td>Oldest month in the result window, formatted as <code>YYYY-MM</code>.</td></tr><tr><td><code>data.toMonth</code></td><td>String</td><td>Current month, formatted as <code>YYYY-MM</code>.</td></tr><tr><td><code>data.monthCount</code></td><td>Number</td><td>Total number of months in the <code>data</code> array.</td></tr><tr><td><code>data.timezone</code></td><td>String</td><td>Timezone applied to all month boundaries — always UTC.</td></tr><tr><td><code>data.data[].month</code></td><td>String</td><td>Month identifier, formatted as <code>YYYY-MM</code>.</td></tr><tr><td><code>data.data[].totalOrder</code></td><td>Number</td><td>Total number of orders placed in the month.</td></tr><tr><td><code>data.data[].totalOrderEnergy</code></td><td>Number</td><td>Number of Energy orders placed in the month.</td></tr><tr><td><code>data.data[].totalOrderBW</code></td><td>Number</td><td>Number of Bandwidth orders placed in the month.</td></tr><tr><td><code>data.data[].totalTrxDepositToCreateBuyOrder</code></td><td>Number</td><td>Total TRX spent to create buy orders in the month.</td></tr><tr><td><code>data.data[].totalTrxDepositInternal</code></td><td>Number</td><td>Total TRX deposited into the internal account in the month.</td></tr></tbody></table>

### 200: OK

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "fromMonth": string, // oldest month in the result window, formatted as YYYY-MM
        "toMonth": string, // current month, formatted as YYYY-MM
        "monthCount": number, // Total number of months in the data array
        "timezone": string, // Timezone applied to all month boundaries — always UTC
        "data": [
            {
                "month": string, // Month identifier, formatted as YYYY-MM
                "totalOrder": number, // Total number of orders placed in the month
                "totalOrderEnergy": number, // Number of energy orders placed in the month
                "totalOrderBW": number, // Number of bandwidth orders placed in the month
                "totalTrxDepositToCreateBuyOrder": number, // Total TRX spent to create buy orders in the month
                "totalTrxDepositInternal": number // Total TRX deposited into the internal account in the month
            }
        ]
    }
}
```

### Success example

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "fromMonth": "2025-12",
        "toMonth": "2026-05",
        "monthCount": 6,
        "timezone": "UTC",
        "data": [
            {
                "month": "2026-05",
                "totalOrder": 16,
                "totalOrderEnergy": 13,
                "totalOrderBW": 3,
                "totalTrxDepositToCreateBuyOrder": 1080.88,
                "totalTrxDepositInternal": 2000
            },
            {
                "month": "2026-04",
                "totalOrder": 23,
                "totalOrderEnergy": 23,
                "totalOrderBW": 0,
                "totalTrxDepositToCreateBuyOrder": 99.6,
                "totalTrxDepositInternal": 164
            }
        ]
    }
}
```

### Errors

{% tabs %}
{% tab title="401 401: Missing API key " %}
The `apikey` The header was not provided.

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```
{% endtab %}

{% tab title="401: Invalid API key" %}
The supplied API key is invalid

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```
{% endtab %}

{% tab title="403 Forbidden" %}
Its wallet address is not whitelisted.

```json
{
    "error": true,
    "message": "TSAS:108 FORBIDDEN Forbidden",
    "data": null
}
```
{% endtab %}
{% endtabs %}



## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.tronsave.io/v2/internal/buyer/monthly-stats" \
  -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API_URL = "https://api.tronsave.io";

const getMonthlyStats = async (apiKey) => {
  const url = `${TRONSAVE_API_URL}/v2/internal/buyer/monthly-stats`;
  const res = await fetch(url, {
    headers: {
      apikey: apiKey,
    },
  });
  return res.json();
};

getMonthlyStats("YOUR_API_KEY").then(console.log);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"
API_KEY = "YOUR_API_KEY"


def get_monthly_stats() -> dict:
    """Get buyer monthly statistics (last 6 months)."""
    url = f"{TRONSAVE_API_URL}/v2/internal/buyer/monthly-stats"
    headers = {"apikey": API_KEY}
    response = requests.get(url, headers=headers)
    return response.json()


print(get_monthly_stats())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetMonthlyStats {
    static final String TRONSAVE_API_URL = "https://api.tronsave.io";
    static final String API_KEY = "YOUR_API_KEY";

    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(TRONSAVE_API_URL + "/v2/internal/buyer/monthly-stats"))
                .header("apikey", API_KEY)
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

const (
	tronsaveAPIURL = "https://api.tronsave.io"
	apiKey         = "YOUR_API_KEY"
)

func main() {
	req, err := http.NewRequest(http.MethodGet, tronsaveAPIURL+"/v2/internal/buyer/monthly-stats", nil)
	if err != nil {
		panic(err)
	}
	req.Header.Set("apikey", apiKey)

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
use serde_json::Value;

const TRONSAVE_API_URL: &str = "https://api.tronsave.io";
const API_KEY: &str = "YOUR_API_KEY";

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::blocking::Client::new();
    let resp: Value = client
        .get(format!("{TRONSAVE_API_URL}/v2/internal/buyer/monthly-stats"))
        .header("apikey", API_KEY)
        .send()?
        .json()?;

    println!("{resp:#?}");
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* [Get Internal Account Info](get-account-info.md) — check your internal account balance and deposit address.
* [Get Order Book](get-order-book.md) — fetch current resource pricing and availability.
* [Authentication](../../../authentication.md) — how to pass your API key on each request.
