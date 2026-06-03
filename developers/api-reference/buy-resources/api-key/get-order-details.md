---
description: Retrieve detailed information and the current status of a specific order using its orderId.
---

# Get Order Details

Retrieve detailed information and the current status of a specific order using its `orderId`. The response includes the order parameters, fulfillment progress, and every on-chain delegate that has matched the order.

## Endpoint

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v2/order/:id`**

The `:id` path segment is the `orderId` returned when the order was created.

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

## Path parameters

<table>
<thead>
<tr><th width="120">Name</th><th width="100">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>id</code><mark style="color:red;">*</mark></td><td>String</td><td>The <code>orderId</code> of the order to retrieve.</td></tr>
</tbody>
</table>

## Request body

This endpoint takes no request body — pass the `apikey` header and the order `id` in the path.

## Response

A successful response returns `error: false` and the order details in `data`.

<table>
<thead>
<tr><th width="220">Field</th><th width="110">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>data.id</code></td><td>String</td><td>ID of the order.</td></tr>
<tr><td><code>data.requester</code></td><td>String</td><td>The address that represents the order owner.</td></tr>
<tr><td><code>data.receiver</code></td><td>String</td><td>The address that receives the resource.</td></tr>
<tr><td><code>data.resourceAmount</code></td><td>Number</td><td>The amount of resource.</td></tr>
<tr><td><code>data.resourceType</code></td><td>String</td><td>The resource type, either <code>ENERGY</code> or <code>BANDWIDTH</code>.</td></tr>
<tr><td><code>data.remainAmount</code></td><td>Number</td><td>The remaining amount that can still be matched by the system.</td></tr>
<tr><td><code>data.price</code></td><td>Number</td><td>Price, in SUN.</td></tr>
<tr><td><code>data.durationSec</code></td><td>Number</td><td>Rent duration, in seconds.</td></tr>
<tr><td><code>data.orderType</code></td><td>String</td><td>Type of order, either <code>NORMAL</code> or <code>EXTEND</code>.</td></tr>
<tr><td><code>data.allowPartialFill</code></td><td>Boolean</td><td>Whether the order may be filled partially.</td></tr>
<tr><td><code>data.payoutAmount</code></td><td>Number</td><td>Total payout of this order.</td></tr>
<tr><td><code>data.fulfilledPercent</code></td><td>Number</td><td>The fill progress as a percentage, 0–100.</td></tr>
<tr><td><code>data.delegates</code></td><td>Array</td><td>All matched delegates for this order.</td></tr>
<tr><td><code>data.delegates[].delegator</code></td><td>String</td><td>The address that delegates the resource to the target address.</td></tr>
<tr><td><code>data.delegates[].amount</code></td><td>Number</td><td>The amount of resource that was delegated.</td></tr>
<tr><td><code>data.delegates[].txid</code></td><td>String</td><td>The on-chain transaction ID.</td></tr>
</tbody>
</table>

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
        "orderType": string, // type of order is "NORMAL" or "EXTEND"
        "allowPartialFill": boolean, //Allow the order to be filled partially or not
        "payoutAmount": number, // Total payout of this order
        "fulfilledPercent": number, //The percent that shows filling processing. 0-100
        "delegates": [ //All matched delegates for this order
            {
                "delegator": string, // The address that delegates the resource for the target address
                "amount": number, //The amount of resource was delegated
                "txid": number // The transaction ID in on-chain
            }
        ]
    }
}
```

### Success response example

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "id": "6819c7578729a45600f740d1",
        "requester": "TFwUFWr3QV376677Z8VWXxGUAMFSSSSSS",
        "receiver": "TFwUFWr3QV376677Z8VWXxGUAMFSSSSSSS",
        "resourceAmount": 32000,
        "resourceType": "ENERGY",
        "remainAmount": 0,
        "price": 90,
        "durationSec": 300,
        "orderType": "NORMAL",
        "allowPartialFill": false,
        "payoutAmount": 2880000,
        "fulfilledPercent": 100,
        "delegates": [
            {
                "delegator": "THnnMCe67VMDXoivepiA7ZQSB888888",
                "amount": 32000,
                "txid": "19d3fa76a722d6d6e671e6141eb8057760d38d42b353153a3825f19a7d34326f"
            }
        ]
    }
}
```

### Errors

This endpoint authenticates with the `apikey` header. Missing or invalid keys return `401`.

**401 Unauthorized — missing API key**

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```

**401 Unauthorized — invalid API key**

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```

**404 Not Found — wrong route/path**

```json
{
    "message": "Route GET:/v2/order/ not found",
    "error": "Not Found",
    "statusCode": 404
}
```

<!-- [NEEDS CONFIRMATION: the exact response body returned when the orderId does not match an existing order is not documented; the order-not-found business message could not be confirmed against the live testnet] -->

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.tronsave.io/v2/order/YOUR_ORDER_ID" \
  -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API_URL = "https://api.tronsave.io";

const getOrderDetails = async (apiKey, orderId) => {
  const url = `${TRONSAVE_API_URL}/v2/order/${orderId}`;
  const res = await fetch(url, {
    headers: {
      apikey: apiKey,
    },
  });
  return res.json();
};

getOrderDetails("YOUR_API_KEY", "YOUR_ORDER_ID").then(console.log);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"
API_KEY = "YOUR_API_KEY"


def get_order_details(order_id: str) -> dict:
    """Get order details by orderId."""
    url = f"{TRONSAVE_API_URL}/v2/order/{order_id}"
    headers = {"apikey": API_KEY}
    response = requests.get(url, headers=headers)
    return response.json()


print(get_order_details("YOUR_ORDER_ID"))
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetOrderDetails {
    static final String TRONSAVE_API_URL = "https://api.tronsave.io";
    static final String API_KEY = "YOUR_API_KEY";

    public static void main(String[] args) throws Exception {
        String orderId = "YOUR_ORDER_ID";

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(TRONSAVE_API_URL + "/v2/order/" + orderId))
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
	orderID := "YOUR_ORDER_ID"

	req, err := http.NewRequest(http.MethodGet, tronsaveAPIURL+"/v2/order/"+orderID, nil)
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
    let order_id = "YOUR_ORDER_ID";

    let client = reqwest::blocking::Client::new();
    let resp: Value = client
        .get(format!("{TRONSAVE_API_URL}/v2/order/{order_id}"))
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

* [Buy Energy (Create Order)](create-order.md) — place an order paid from your internal account.
* [Get Order Book](get-order-book.md) — fetch current resource pricing and availability.
* [Authentication](../../../authentication.md) — how to pass your API key on each request.
