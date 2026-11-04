---
description: >-
  Retrieve all active buy orders currently waiting to be fulfilled on the
  TronSave marketplace.
---

# Get active orders

{% hint style="warning" %}
**Whitelist required.** The API is restricted to whitelisted wallets. Log in to TronSave and send your wallet address to support to add it to the whitelist. Once whitelisted, use the API Key generated from that wallet address to call the Sell endpoints.

Contact support on Telegram: [**@wantingtrx**](https://t.me/wantingtrx)
{% endhint %}

## Endpoint

Returns the global list of active buy orders available for matching.

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v2/orders/active-global`**

{% hint style="info" %}
**Rate limit:** 3 requests per 1 second.
{% endhint %}

### Headers

<table><thead><tr><th width="160">Name</th><th width="120">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code> <mark style="color:red;">*</mark></td><td>String</td><td>TronSave API Key (must belong to a whitelisted address). See <a href="../../authentication.md">Authentication</a>.</td></tr></tbody></table>

### Query parameters

<table><thead><tr><th width="221">Name</th><th width="172">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>page</code></td><td>Integer</td><td>Page index, starting from 0. Default: <code>0</code></td></tr><tr><td><code>pageSize</code></td><td>Integer</td><td>Number of orders per page. Default: <code>10</code></td></tr></tbody></table>

### Response

```javascript
{
  "error": false,
  "message": "Success",
  "data": {
    "data": [
      {
        "id": string,            // Order identifier — use as orderId in Step 2
        "receiver": string,      // TRON address that receives the delegated resource
        "resourceAmount": number,// the amount of resource
        "resourceType": string,  // the resource type is "ENERGY" or "BANDWIDTH"
        "remainAmount": number,  // the remaining amount that can be matched by the system
        "orderType": string,     // type of order is "NORMAL" or "EXTEND"
        "price": number,         // price unit is equal to SUN
        "durationSec": number,   // rent duration, duration unit is equal to seconds
        "status": string,        // the order status, either Active or Completed
        "allowPartialFill": boolean, // Allow the order to be filled partially or not
        "payoutAmount": number,  // Total payout of this order (SUN)
        "fulfilledPercent": number, // The percent that shows filling progress. 0-100
        "createdAt": 1778467289
      }
    ],
    "total": 3
  }
}
```

**Example response**

```javascript
{
  "error": false,
  "message": "Success",
  "data": {
    "data": [
      {
        "id": "6a0141d98e6f3f91d2284444",
        "receiver": "TA4JgKPtrPLtGQ1WMz3qmorqbL22221111",
        "resourceAmount": 500000,
        "resourceType": "ENERGY",
        "remainAmount": 339999,
        "orderType": "NORMAL",
        "price": 50,
        "durationSec": 259200,
        "status": "Active",
        "allowPartialFill": true,
        "payoutAmount": 24000150,
        "fulfilledPercent": 32,
        "createdAt": 1778467289
      }
    ],
    "total": 3
  }
}
```

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET \
  "https://api.tronsave.io/v2/orders/active-global?page=0&pageSize=10" \
  -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const res = await fetch(
  "https://api.tronsave.io/v2/orders/active-global?page=0&pageSize=10",
  {
    method: "GET",
    headers: {
      apikey: "YOUR_API_KEY",
      "content-type": "application/json",
    },
  }
);

const data = await res.json();
console.log(data.data.data);
```
{% endtab %}

{% tab title="Python" %}
```python
import urllib.parse
import urllib.request
import json

query = urllib.parse.urlencode({"page": 0, "pageSize": 10})
url = f"https://api.tronsave.io/v2/orders/active-global?{query}"

req = urllib.request.Request(
    url,
    method="GET",
    headers={"apikey": "YOUR_API_KEY", "content-type": "application/json"},
)
with urllib.request.urlopen(req) as response:
    data = json.loads(response.read().decode("utf-8"))
print(data["data"]["data"])
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetActiveOrders {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.tronsave.io/v2/orders/active-global?page=0&pageSize=10"))
            .header("apikey", "YOUR_API_KEY")
            .header("content-type", "application/json")
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
	url := "https://api.tronsave.io/v2/orders/active-global?page=0&pageSize=10"
	req, _ := http.NewRequest("GET", url, nil)
	req.Header.Set("apikey", "YOUR_API_KEY")
	req.Header.Set("content-type", "application/json")

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
// Cargo.toml:
// reqwest = { version = "0.12", features = ["blocking", "json"] }
use std::error::Error;

fn main() -> Result<(), Box<dyn Error>> {
    let client = reqwest::blocking::Client::new();
    let res = client
        .get("https://api.tronsave.io/v2/orders/active-global")
        .query(&[("page", "0"), ("pageSize", "10")])
        .header("apikey", "YOUR_API_KEY")
        .header("content-type", "application/json")
        .send()?;

    let body: serde_json::Value = res.json()?;
    println!("{}", body);
    Ok(())
}
```
{% endtab %}
{% endtabs %}



## Next steps

* [Sell the resources ](sell-resources-1.md)- submit a signed delegate transaction against a chosen order.
* [Get Delegate Detail ](sell-resources-2.md)- get detailed information and the current status of a specific delegate using its delegateId.

