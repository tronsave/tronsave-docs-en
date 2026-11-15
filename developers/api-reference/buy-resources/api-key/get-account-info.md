---
description: Retrieve the internal account information associated with your TronSave API key, including balance and deposit address.
---

# Get Internal Account Info

Retrieve internal account information associated with the provided API key, including the account `id`, current `balance` in SUN, the address that represents the account on orders, and the address to deposit TRX into.

## Endpoint

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v2/user-info`**

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

## Request body

This endpoint takes no request body — pass the `apikey` header only.

## Response

A successful response returns `error: false` and the account details in `data`.

<table>
<thead>
<tr><th width="180">Field</th><th width="100">Type</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td><code>data.id</code></td><td>String</td><td>Internal account ID.</td></tr>
<tr><td><code>data.balance</code></td><td>String</td><td>Internal account balance, in SUN.</td></tr>
<tr><td><code>data.representAddress</code></td><td>String</td><td>Represents the internal account as the requester of the order.</td></tr>
<tr><td><code>data.depositAddress</code></td><td>String</td><td>Send TRX to this address to deposit into your internal account.</td></tr>
</tbody>
</table>

### 200: OK

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "id": string, //internal account id
        "balance": string, //internal account balance in SUN
        "representAddress": string, //represents the internal account as the requester of the order
        "depositAddress": string, //Send TRX to this address to deposit into your internal account.
    }
}
```

### Success response example

```json
{
    "error": false,
    "message": "Success",
    "data": {
        "id": "user_id",
        "balance": "306773887",
        "representAddress": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
        "depositAddress": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999"
    }
}
```

### Errors

This endpoint authenticates with the `apikey` header, so the common errors are authentication failures.

**401 Unauthorized — missing API key** (no `apikey` header sent):

```json
{
    "error": true,
    "message": "TSAS:106 API_KEY_REQUIRED",
    "data": null
}
```

**401 Unauthorized — invalid API key**:

```json
{
    "error": true,
    "message": "TSAS:107 INVALID_API_KEY",
    "data": null
}
```

## Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.tronsave.io/v2/user-info" \
  -H "apikey: YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const TRONSAVE_API_URL = "https://api.tronsave.io";

const getAccountInfo = async (apiKey) => {
  const url = `${TRONSAVE_API_URL}/v2/user-info`;
  const res = await fetch(url, {
    headers: {
      apikey: apiKey,
    },
  });
  return res.json();
};

getAccountInfo("YOUR_API_KEY").then(console.log);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TRONSAVE_API_URL = "https://api.tronsave.io"
API_KEY = "YOUR_API_KEY"


def get_account_info() -> dict:
    """Get internal account information."""
    url = f"{TRONSAVE_API_URL}/v2/user-info"
    headers = {"apikey": API_KEY}
    response = requests.get(url, headers=headers)
    return response.json()


print(get_account_info())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetAccountInfo {
    static final String TRONSAVE_API_URL = "https://api.tronsave.io";
    static final String API_KEY = "YOUR_API_KEY";

    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(TRONSAVE_API_URL + "/v2/user-info"))
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
	req, err := http.NewRequest(http.MethodGet, tronsaveAPIURL+"/v2/user-info", nil)
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
        .get(format!("{TRONSAVE_API_URL}/v2/user-info"))
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

* [Get Order Book](get-order-book.md) — fetch current resource pricing and availability.
* [Buy Energy (Create Order)](create-order.md) — place an order paid from your internal account.
* [Authentication](../../../authentication.md) — how to pass your API key on each request.
