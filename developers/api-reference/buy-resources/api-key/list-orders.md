---
description: Retrieve a detailed, paginated list of orders created by a whitelisted buyer account, authenticated with a TronSave API key.
---

# List Orders

Retrieve a detailed list of orders created by the buyer account. This endpoint returns extended order data for advanced integrations and analytics. It requires API key whitelist access.

{% hint style="warning" %}
**Whitelist required**\
Log in to TronSave and send your wallet address to our support team so it can be added to the whitelist. Once whitelisted, you can use the API key generated from that wallet address to call this endpoint.

Contact support via Telegram: [**@wantingtrx**](https://t.me/wantingtrx).
{% endhint %}

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v2/internal/buyer/orders`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

## Headers

<table>
<thead>
<tr><th width="140">Name</th><th width="100">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key (must belong to a whitelisted address). See <a href="../../../authentication.md">Authentication</a>.</td></tr>
</tbody>
</table>

<mark style="color:red;">*</mark> Required.

## Query parameters

<table>
<thead>
<tr><th width="170">Name</th><th width="100">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>startTimestamp</code></td><td>Number</td><td>Timestamp in seconds: start of the time range to filter orders.</td></tr>
<tr><td><code>endTimestamp</code></td><td>Number</td><td>Timestamp in seconds: end of the time range to filter orders.</td></tr>
<tr><td><code>pageSize</code></td><td>Number</td><td>Number of records per page. (Default: 20, Maximum: 20)</td></tr>
<tr><td><code>cursor</code></td><td>String</td><td>Pagination cursor. Use <code>nextCursor</code> to go forward, <code>prevCursor</code> to go backward. (Omit to fetch the first page.)</td></tr>
<tr><td><code>direction</code></td><td>String</td><td>Only needed when navigating <strong>backward</strong>: pass <code>direction=prev</code> together with <code>cursor=prevCursor</code>. To go forward, just pass <code>cursor=nextCursor</code> — no <code>direction</code> needed.</td></tr>
</tbody>
</table>

## Response

The `data` object wraps the paginated result. Each entry in the inner `data` array is one order. Use `id` as the `orderId` in subsequent calls. `price` and `payoutAmount` are in SUN; `durationSec` is in seconds; `fulfilledPercent` ranges from 0 to 100.

```javascript
{
    "error": false,
    "message": "Success",
    "data": {
        "startTimestamp": number,// Start of the time range to filter orders.
        "endTimestamp": number,// End of the time range to filter orders.
        "data": [
        {
          "id": string,//Order identifier — use as orderId in Step 2
          "receiver": string,//TRON address that receives the delegated resource
          "resourceAmount": number, // the amount of resource
          "resourceType": string, // the resource type is "ENERGY" or "BANDWIDTH"
          "remainAmount": number,// the remaining amount can be matched by the system
          "orderType":  string, // type of order is "NORMAL" or "EXTEND"
          "price": number, // price unit is equal to SUN
          "durationSec": number, // rent duration, duration unit is equal to seconds
          "status": string, // the order status, either Active or Completed
          "allowPartialFill": boolean, //Allow the order to be filled partially or not
          "payoutAmount": number, // Total payout of this order (SUN)
          "fulfilledPercent":  number, //The percent that shows filling processing. 0-100
          "smartMatching": {object}, // Contains smart matching details (order count, matched amount, and refund amount)
          "totalMergeInfo": {object}, // Contains merge summary (merged amount and merged TRX)
          "extendInfo": [], // Lists extend delegation records (delegator, amount, payout, etc.)
          "createdAt": 1778467289
        }
      ],
        "pageSize": number, // Number of records per page
        "prevCursor": string, // Pass as cursor with direction=prev to fetch the previous page.
        "nextCursor": string // Pass as cursor to fetch the next page. null when there are no more pages ahead.
    }
}
```

{% tabs %}
{% tab title="200: OK" %}
```json
{
    "error": false,
    "message": "Success",
    "data": {
        "startTimestamp": 1777507200,
        "endTimestamp": 1779349961,
        "data": [
            {
                "id": "6a0eb6708f648f498632acee",
                "receiver": "TFwUFWr3QV376677Z8VWXxGUAMFSrq1111",
                "resourceAmount": 66666,
                "resourceType": "ENERGY",
                "remainAmount": 0,
                "orderType": "NORMAL",
                "price": 65,
                "durationSec": 900,
                "status": "Completed",
                "allowPartialFill": false,
                "payoutAmount": 4333290,
                "fulfilledPercent": 100,
                "createdAt": 1779349104
            },
            {
                "id": "6a02e2a5302419258a85d524",
                "receiver": "TUP4FoZGxFZTXzCPZmfuKdGniB8J8eAAAA",
                "resourceAmount": 1000,
                "resourceType": "BANDWIDTH",
                "remainAmount": 0,
                "orderType": "NORMAL",
                "price": 600,
                "durationSec": 900,
                "status": "Completed",
                "allowPartialFill": false,
                "payoutAmount": 600000,
                "fulfilledPercent": 100,
                "createdAt": 1778573989
            }
        ],
        "pageSize": 2,
        "prevCursor": "af28gmn9vIINZjkQk46sf-bkhEM",
        "nextCursor": "af28dGn9vHQNZjkQk46sfUTFQXU"
    }
}
```
{% endtab %}

{% tab title="401: Missing API key" %}
The `apikey` header was not provided.

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```
{% endtab %}

{% tab title="401: Invalid API key" %}
The supplied API key is invalid, or its wallet address is not whitelisted.

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```
{% endtab %}
{% endtabs %}

## Example

Query parameters:

<table>
<thead>
<tr><th width="263">Key</th><th>Value</th></tr>
</thead>
<tbody>
<tr><td><code>startTimestamp</code></td><td>1778236532</td></tr>
<tr><td><code>endTimestamp</code></td><td>1779349961</td></tr>
<tr><td><code>pageSize</code></td><td>2</td></tr>
<tr><td><code>cursor</code></td><td>af28gmn9vIINZjkQk46sf-bkhEM</td></tr>
</tbody>
</table>

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.tronsave.io/v2/internal/buyer/orders?startTimestamp=1778236532&endTimestamp=1779349961&pageSize=2&cursor=af28gmn9vIINZjkQk46sf-bkhEM" \
  -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API_URL = "https://api.tronsave.io";

const listBuyerOrders = async (apiKey, params = {}) => {
  const query = new URLSearchParams(params).toString();
  const url = `${TRONSAVE_API_URL}/v2/internal/buyer/orders?${query}`;
  const res = await fetch(url, {
    method: "GET",
    headers: {
      apikey: apiKey,
    },
  });
  return res.json();
};

listBuyerOrders("YOUR_API_KEY", {
  startTimestamp: 1778236532,
  endTimestamp: 1779349961,
  pageSize: 2,
  cursor: "af28gmn9vIINZjkQk46sf-bkhEM",
}).then(console.log);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"

def list_buyer_orders(api_key: str, params: dict) -> dict:
    url = f"{TRONSAVE_API_URL}/v2/internal/buyer/orders"
    headers = {"apikey": api_key}

    response = requests.get(url, headers=headers, params=params)
    return response.json()

print(list_buyer_orders("YOUR_API_KEY", {
    "startTimestamp": 1778236532,
    "endTimestamp": 1779349961,
    "pageSize": 2,
    "cursor": "af28gmn9vIINZjkQk46sf-bkhEM",
}))
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class ListBuyerOrders {
    public static void main(String[] args) throws Exception {
        String apiKey = "YOUR_API_KEY";
        String url = "https://api.tronsave.io/v2/internal/buyer/orders"
                + "?startTimestamp=1778236532"
                + "&endTimestamp=1779349961"
                + "&pageSize=2"
                + "&cursor=af28gmn9vIINZjkQk46sf-bkhEM";

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .header("apikey", apiKey)
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
	apiKey := "YOUR_API_KEY"
	url := "https://api.tronsave.io/v2/internal/buyer/orders" +
		"?startTimestamp=1778236532" +
		"&endTimestamp=1779349961" +
		"&pageSize=2" +
		"&cursor=af28gmn9vIINZjkQk46sf-bkhEM"

	req, err := http.NewRequest(http.MethodGet, url, nil)
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

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let api_key = "YOUR_API_KEY";
    let url = "https://api.tronsave.io/v2/internal/buyer/orders";

    let client = reqwest::blocking::Client::new();
    let resp: Value = client
        .get(url)
        .header("apikey", api_key)
        .query(&[
            ("startTimestamp", "1778236532"),
            ("endTimestamp", "1779349961"),
            ("pageSize", "2"),
            ("cursor", "af28gmn9vIINZjkQk46sf-bkhEM"),
        ])
        .send()?
        .json()?;

    println!("{:#?}", resp);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Next steps

* [Create an order](create-order.md) to buy Energy or Bandwidth.
* [Get the order book](get-order-book.md) to inspect pricing before ordering.
* Learn the difference between [Energy and Bandwidth](../../../../concepts/energy-and-bandwidth.md).
