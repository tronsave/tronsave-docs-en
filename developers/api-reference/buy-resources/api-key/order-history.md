---
description: Retrieve the order history of the internal account associated with your TronSave API key, with pagination and status filtering.
---

# Get Internal Account Order History

Retrieve the order history of the internal account associated with the provided API key. Orders are returned sorted by creation time, newest first. By default the endpoint returns the 10 newest orders.

## Endpoint

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v2/orders`**

{% hint style="info" %}
**Rate limit:** 15 requests per 1 second.
{% endhint %}

## Headers

<table>
<thead>
<tr><th width="120">Name</th><th width="100">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that represents the internal account. See <a href="../../../authentication.md">Authentication</a> to get your API key.</td></tr>
</tbody>
</table>

<mark style="color:red;">*</mark> Required.

## Query parameters

<table>
<thead>
<tr><th width="200">Name</th><th width="156">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>page</code></td><td>Integer</td><td>Page index, starts from 0. Default: <code>0</code>.</td></tr>
<tr><td><code>pageSize</code></td><td>Integer</td><td>Number of orders per page. Default: <code>10</code>.</td></tr>
<tr><td><code>status</code></td><td>String</td><td><code>"Active"</code> or <code>"Completed"</code>. Default: get all.</td></tr>
</tbody>
</table>

## Request body

This endpoint takes no request body — pass the `apikey` header and optional query parameters only.

## Response

A successful response returns `error: false` and the matching orders in `data.data`, along with the total count in `data.total`.

### 200: OK

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "id":  string, // id of order
        "requester": string, // the address represents the order owner
        "receiver": string, // the address of the resource that is received 
        "resourceAmount": number, // the amount of resource
        "resourceType": string, // the resource type is "ENERGY" or "BANDWIDTH"
        "remainAmount": number, // the remaining amount can be matched by the system,
        "price": number, // price unit is equal to SUN
        "durationSec": number, // rent duration, duration unit is equal to seconds
        "status": string, // the order status, either Active or Completed
        "orderType": string, // type of order is "NORMAL" or "EXTEND"
        "allowPartialFill": boolean, //Allow the order to be filled partially or not
        "payoutAmount": number, // Total payout of this order
        "fulfilledPercent": number, //The percent that shows filling processing. 0-100
        "delegates": [ //All matched delegates for this order
            {
                "delegator": string, // The address that delegates the resource for the target address
                "amount": number, //The amount of resource was delegated
                "txid": number // The transaction ID in on-chain
             }[]
       } 
    }[],
        "total": number
}
```

### Success response example

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "data": [
            {
                "id": "6819c7578729a45600f740d3",
                "requester": "TFwUFWr3QV376677Z8VWXxGUAMFSSSSSS",
                "receiver": "TFwUFWr3QV376677Z8VWXxGUAMFSSSSSS",
                "resourceAmount": 32000,
                "resourceType": "ENERGY",
                "remainAmount": 0,
                "orderType": "NORMAL",
                "price": 90,
                "durationSec": 300,
                "allowPartialFill": false,
                "payoutAmount": 2880000,
                "fulfilledPercent": 100,
                "delegates": [
                    {
                        "delegator": "THnnMCe67VMDXoivepiA7ZQSB8888888",
                        "amount": 32000,
                        "txid": "transaction_id_1"
                    }
                ]
            },
            {
                "id": "68198bcd8729a45600f740cf",
                "requester": "TFwUFWr3QV376677Z8VWXxGUAMFSSSSSS",
                "receiver": "TFwUFWr3QV376677Z8VWXxGUAMFSSSSSS",
                "resourceAmount": 1234,
                "resourceType": "BANDWIDTH",
                "remainAmount": 0,
                "orderType": "NORMAL",
                "price": 600,
                "durationSec": 900,
                "allowPartialFill": false,
                "payoutAmount": 740400,
                "fulfilledPercent": 100,
                "delegates": [
                    {
                        "delegator": "TMhiksDwSVjuxdXLwdNQEJpuFCLG77777",
                        "amount": 1234,
                        "txid": "transaction_id_2"
                    }
                ]
            }
        ],
        "total": 2
    }
}
```

### Error responses

This endpoint authenticates with the `apikey` header. Missing or invalid keys return `401`.

**401 Unauthorized — missing API key:**

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```

**401 Unauthorized — invalid API key:**

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```

**400 Bad Request — schema validation.** Invalid query parameters are rejected. The `message` names the offending field, for example:

```json
{
    "statusCode": 400,
    "code": "FST_ERR_VALIDATION",
    "error": "Bad Request",
    "message": "querystring/status must be equal to one of the allowed values"
}
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.tronsave.io/v2/orders?page=0&pageSize=10" \
  -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API_URL = "https://api.tronsave.io";

const getOrderHistory = async (apiKey) => {
  const url = `${TRONSAVE_API_URL}/v2/orders`;
  const res = await fetch(url, {
    headers: {
      apikey: apiKey,
    },
  });
  return res.json();
};

getOrderHistory("YOUR_API_KEY").then(console.log);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"
API_KEY = "YOUR_API_KEY"


def get_order_history() -> dict:
    """Get order history for the internal account."""
    url = f"{TRONSAVE_API_URL}/v2/orders"
    headers = {"apikey": API_KEY}
    response = requests.get(url, headers=headers)
    return response.json()


print(get_order_history())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetOrderHistory {
    static final String TRONSAVE_API_URL = "https://api.tronsave.io";
    static final String API_KEY = "YOUR_API_KEY";

    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(TRONSAVE_API_URL + "/v2/orders"))
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
	req, err := http.NewRequest(http.MethodGet, tronsaveAPIURL+"/v2/orders", nil)
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
        .get(format!("{TRONSAVE_API_URL}/v2/orders"))
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

* [Get Internal Account Info](get-account-info.md) — check your balance and deposit address.
* [Buy Energy (Create Order)](create-order.md) — place a new order paid from your internal account.
* [Authentication](../../../authentication.md) — how to pass your API key on each request.
