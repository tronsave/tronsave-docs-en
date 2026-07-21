---
description: >-
  Legacy v0 REST endpoints for buying Energy with an API key — account info,
  order book, TRX estimate, create order, order details, order history, and
  wallet activation.
---

# Buy — API Key

{% hint style="warning" %}
This page documents the **legacy v0 API**. New integrations should use the current API. See [Buy with API Key](../../api-reference/buy-resources/api-key/) for the supported endpoints. The v0 endpoints below remain available for existing integrations.
{% endhint %}

All endpoints on this page authenticate with an **API key** sent in the `apikey` header. The key is tied to your TronSave [internal account](../../authentication.md), and orders are paid from that account's balance. See [Authentication](../../authentication.md) to get a key.

**Base URL (mainnet):** `https://api.tronsave.io`

{% hint style="info" %}
**TRON Nile Testnet:** replace the base URL with `https://api-dev.tronsave.io`.

* Estimate TRX: <mark style="color:orange;">`POST`</mark> `https://api-dev.tronsave.io/v0/estimate-trx`
* Create order: <mark style="color:orange;">`POST`</mark> `https://api-dev.tronsave.io/v0/internal-buy-energy`
* Get Internal Account Info: <mark style="color:blue;">`GET`</mark> `https://api-dev.tronsave.io/v0/user-info`
* Get Internal Account Order History: <mark style="color:blue;">`GET`</mark> `https://api-dev.tronsave.io/v0/orders`
* Get one order details: <mark style="color:blue;">`GET`</mark> `https://api-dev.tronsave.io/v0/orders/:id`
* Get Order Book: <mark style="color:blue;">`GET`</mark> `https://api-dev.tronsave.io/v0/order-book`
{% endhint %}

Every endpoint below is rate limited to **15** requests per **1** second.

***

## Get Internal Account Info

Get account info by API key.

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v0/user-info`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

### Headers

<table><thead><tr><th width="150">Name</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that represents your internal account.</td></tr></tbody></table>

<sub>\* Required.</sub>

### Response

<table><thead><tr><th width="220">Field</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td>string</td><td>Internal account id.</td></tr><tr><td><code>balance</code></td><td>string</td><td>Internal account balance in SUN.</td></tr><tr><td><code>represent_address</code></td><td>string</td><td>Represents the internal account as the requester of the order.</td></tr><tr><td><code>deposit_address</code></td><td>string</td><td>Deposit address of the internal account.</td></tr></tbody></table>

```json
{
    "id": "user_id",
    "balance": "1000000",
    "represent_address": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
    "deposit_address": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999"
}
```

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://api.tronsave.io/v0/user-info' \
  --header 'apikey: YOUR_API_KEY'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const getAccountInfo = async () => {
  const url = "https://api.tronsave.io/v0/user-info";
  const res = await fetch(url, {
    headers: { apikey: "YOUR_API_KEY" },
  });
  const response = await res.json();
  // {
  //   "id": "user_id",
  //   "balance": "1000000",
  //   "represent_address": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
  //   "deposit_address": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999"
  // }
  return response;
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v0/user-info"
headers = {"apikey": "YOUR_API_KEY"}

response = requests.get(url, headers=headers)
print(response.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetAccountInfo {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.tronsave.io/v0/user-info"))
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
	req, _ := http.NewRequest("GET", "https://api.tronsave.io/v0/user-info", nil)
	req.Header.Set("apikey", "YOUR_API_KEY")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	out, _ := io.ReadAll(resp.Body)
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::blocking::Client::new();
    let response = client
        .get("https://api.tronsave.io/v0/user-info")
        .header("apikey", "YOUR_API_KEY")
        .send()?;

    println!("{}", response.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

***

## Get Order Book

Get the order book by API key.

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v0/order-book`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

### Headers

<table><thead><tr><th width="150">Name</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that represents your internal account.</td></tr></tbody></table>

<sub><mark style="color:red;">\*<mark style="color:red;"></sub> <sub>Required.</sub>

### Query parameters

<table><thead><tr><th width="220">Name</th><th width="108">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>address</code></td><td>string</td><td>Energy receiver address.</td></tr><tr><td><code>min_delegate_amount</code></td><td>number</td><td>The minimum amount of Energy delegated from one provider.</td></tr><tr><td><code>duration_sec</code></td><td>number</td><td>Order duration in seconds.</td></tr></tbody></table>

### Response

```typescript
{
    price: number, // price in SUN
    available_energy_amount: number, // available resource amount at this price
}[]
```

```json
[
    {
        "price": -1,
        "available_energy_amount": 177451
    },
    {
        "price": 30,
        "available_energy_amount": 331088
    },
    {
        "price": 35,
        "available_energy_amount": 2841948
    }
]
```

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl 'https://api.tronsave.io/v0/order-book?address=YOUR_TRON_ADDRESS&min_delegate_amount=100000&duration_sec=86400' \
  --header 'apikey: YOUR_API_KEY'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const getOrderBook = async () => {
  const receiverAddress = "YOUR_TRON_ADDRESS";
  const url = `https://api.tronsave.io/v0/order-book?address=${receiverAddress}&min_delegate_amount=100000&duration_sec=86400`;
  const res = await fetch(url, {
    headers: { apikey: "YOUR_API_KEY" },
  });
  return res.json();
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v0/order-book"
headers = {"apikey": "YOUR_API_KEY"}
params = {
    "address": "YOUR_TRON_ADDRESS",
    "min_delegate_amount": 100000,
    "duration_sec": 86400,
}

response = requests.get(url, headers=headers, params=params)
print(response.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetOrderBook {
    public static void main(String[] args) throws Exception {
        String url = "https://api.tronsave.io/v0/order-book"
                + "?address=YOUR_TRON_ADDRESS"
                + "&min_delegate_amount=100000"
                + "&duration_sec=86400";

        HttpClient client = HttpClient.newHttpClient();
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
	url := "https://api.tronsave.io/v0/order-book" +
		"?address=YOUR_TRON_ADDRESS" +
		"&min_delegate_amount=100000" +
		"&duration_sec=86400"

	req, _ := http.NewRequest("GET", url, nil)
	req.Header.Set("apikey", "YOUR_API_KEY")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	out, _ := io.ReadAll(resp.Body)
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::blocking::Client::new();
    let response = client
        .get("https://api.tronsave.io/v0/order-book")
        .header("apikey", "YOUR_API_KEY")
        .query(&[
            ("address", "YOUR_TRON_ADDRESS"),
            ("min_delegate_amount", "100000"),
            ("duration_sec", "86400"),
        ])
        .send()?;

    println!("{}", response.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

***

## Estimate TRX

Estimate the TRX cost for a purchase before creating the order.

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/estimate-trx`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

### Headers

<table><thead><tr><th width="150">Name</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that represents your internal account.</td></tr></tbody></table>

<sub>\* Required.</sub>

### Request body

<table><thead><tr><th width="180">Field</th><th width="95">Position</th><th width="120">Type</th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>amount</code></td><td>body</td><td>number</td><td>true</td><td>The number of resources.</td></tr><tr><td><code>buy_energy_type</code></td><td>body</td><td>string, number</td><td>true</td><td><p><code>"FAST"</code>, <code>"MEDIUM"</code>, <code>"SLOW"</code>, or a number:</p><p><br>- <strong>FAST</strong>: If the market is ready to fill = 100%, FAST = MEDIUM. If the market is ready to fill &#x3C; 100%, FAST = MEDIUM + 10. If market ready to fill = 0%, FAST = SLOW + 20.</p><p>- <strong>MEDIUM</strong>: The lowest price for the maximum market fill for this order. If market ready to fill = 0%, MEDIUM = SLOW + 10.</p><p>- <strong>SLOW</strong>: The lowest price that can be set for this order.</p><p>- If the price is a number, the price unit is SUN.</p></td></tr><tr><td><code>duration_millisec</code></td><td>body</td><td>number</td><td>true</td><td>The duration of the bought resource, in milliseconds.</td></tr><tr><td><code>request_address</code></td><td>body</td><td>string</td><td>false</td><td>The address of the requester.</td></tr><tr><td><code>target_address</code></td><td>body</td><td>string</td><td>false</td><td>The address of the resource receiver.</td></tr><tr><td><code>is_partial</code></td><td>body</td><td>boolean</td><td>false</td><td>Allow the order to be filled partially or not.</td></tr></tbody></table>

#### Request body example

```json
{
      "amount": 100000,
      "buy_energy_type": "MEDIUM",
      "duration_millisec": 259200000
}
```

### Response

<table><thead><tr><th width="185">Field</th><th width="130">Type</th><th width="110">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>unit_price</code></td><td>number</td><td>true</td><td>Price in SUN of Energy that fits your <code>buy_energy_type</code>.</td></tr><tr><td><code>duration_millisec</code></td><td>number</td><td>true</td><td>Duration in milliseconds.</td></tr><tr><td><code>available_energy</code></td><td>number</td><td>true</td><td>Total available Energy on the TronSave market that matches <code>unit_price</code>.</td></tr><tr><td><code>estimate_trx</code></td><td>number</td><td>true</td><td>Estimated total TRX value to pay for all <code>available_energy</code> at <code>unit_price</code> over <code>duration_millisec</code>.</td></tr></tbody></table>

```json
{
    "unit_price": 45,
    "duration_millisec": 259200000,
    "available_energy": 4298470,
    "estimate_trx": 13500000
}
```

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://api.tronsave.io/v0/estimate-trx' \
  --header 'apikey: YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "amount": 100000,
    "buy_energy_type": "MEDIUM",
    "duration_millisec": 259200000
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const estimateTrx = async () => {
  const url = "https://api.tronsave.io/v0/estimate-trx";
  const body = {
    amount: 100000,
    buy_energy_type: "MEDIUM", // price in SUN, or "SLOW" | "MEDIUM" | "FAST"
    duration_millisec: 259200000,
  };
  const res = await fetch(url, {
    method: "POST",
    headers: {
      apikey: "YOUR_API_KEY",
      "content-type": "application/json",
    },
    body: JSON.stringify(body),
  });
  return res.json();
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v0/estimate-trx"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "amount": 100000,
    "buy_energy_type": "MEDIUM",
    "duration_millisec": 259200000,
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
        String url = "https://api.tronsave.io/v0/estimate-trx";
        String body = """
            {
              "amount": 100000,
              "buy_energy_type": "MEDIUM",
              "duration_millisec": 259200000
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
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
	url := "https://api.tronsave.io/v0/estimate-trx"
	body := []byte(`{
		"amount": 100000,
		"buy_energy_type": "MEDIUM",
		"duration_millisec": 259200000
	}`)

	req, _ := http.NewRequest("POST", url, bytes.NewBuffer(body))
	req.Header.Set("apikey", "YOUR_API_KEY")
	req.Header.Set("Content-Type", "application/json")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	out, _ := io.ReadAll(resp.Body)
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "https://api.tronsave.io/v0/estimate-trx";
    let body = json!({
        "amount": 100000,
        "buy_energy_type": "MEDIUM",
        "duration_millisec": 259200000_u64
    });

    let client = reqwest::blocking::Client::new();
    let response = client
        .post(url)
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

***

## Buy Energy (Create Order)

Create a new buy Energy order by API key. The order is paid from your internal account balance.

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/internal-buy-energy`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

### Headers

<table><thead><tr><th width="150">Name</th><th width="140">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that represents your internal account.</td></tr></tbody></table>

<sub>\* Required.</sub>

### Request body

<table><thead><tr><th width="270">Name</th><th width="93">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>resource_type</code><mark style="color:red;">*</mark></td><td>String</td><td><code>"ENERGY"</code>.</td></tr><tr><td><code>buy_energy_type</code><mark style="color:red;">*</mark></td><td>String</td><td><p>- <strong>FAST</strong>: If the market is ready to fill = 100%, FAST = MEDIUM. If the market is ready to fill &#x3C; 100%, FAST = MEDIUM + 10. If market ready to fill = 0%, FAST = SLOW + 20.</p><p>- <strong>MEDIUM</strong>: The lowest price for the maximum market fill for this order. If market ready to fill = 0%, MEDIUM = SLOW + 10.</p><p>- <strong>SLOW</strong>: The lowest price that can be set for this order.</p><p>- If the price is a number, the price unit is SUN.</p></td></tr><tr><td><code>amount</code><mark style="color:red;">*</mark></td><td>Number</td><td>Amount of resource to buy.</td></tr><tr><td><code>allow_partial_fill</code><mark style="color:red;">*</mark></td><td>Boolean</td><td>If <code>true</code>, the order can be filled from many delegators, making it easier to fill than when <code>false</code>. Amounts greater than 200k Energy can set this parameter.</td></tr><tr><td><code>target_address</code><mark style="color:red;">*</mark></td><td>String</td><td>The address that receives the resource.</td></tr><tr><td><code>duration_millisec</code></td><td>Number</td><td>Order duration in milliseconds. Default: 259200000 (3 days).</td></tr><tr><td><code>sponsor</code></td><td>String</td><td>Sponsor code.</td></tr><tr><td><code>only_create_when_fulfilled</code></td><td>Boolean</td><td><p><code>true</code> => order only creates when it can be fulfilled.</p><p><code>false</code> => order will create even if it cannot be fulfilled.</p><p>Default value: <code>false</code>.</p></td></tr><tr><td><code>max_price_accepted</code></td><td>Number</td><td>Only create an order when the estimated price is less than this value.</td></tr><tr><td><code>add_order_incomplete</code></td><td>Boolean</td><td><p><code>true</code> => order only creates when there is no incomplete order with the same parameters in the order list.</p><p><code>false</code> => order will create even when there is no incomplete order with the same parameters in the order list.</p><p>Default value: <code>false</code>.</p></td></tr></tbody></table>

<sub>\* Required.</sub>

#### Request body example

```json
{
    "resource_type": "ENERGY",
    "buy_energy_type": "FAST",
    "amount": 100000,
    "allow_partial_fill": true,
    "target_address": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
    "duration_millisec": 86400000,
    "only_create_when_fulfilled": false,
    "max_price_accepted": 100,
    "add_order_incomplete": false
}
```

### Responses

{% tabs %}
{% tab title="200: OK Success" %}
Returns the order id on success.

```json
{
      "order_id": "651d2306e55c073f6ca0992e",
      "requester": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
      "target": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
      "resource_amount": 100000,
      "resource_type": "ENERGY",
      "remain_amount": 0,
      "price": 67.5,
      "duration": 3600,
      "allow_partial_fill": true,
      "payout_amount": 6750000,
      "fulfilled_percent": 100
}
```
{% endtab %}

{% tab title="400: Bad Request" %}
```json
{
    "MISSING_PARAMS": "Missing some params in body",
    "INVALID_PARAMS": "some params is invalid",
    "ORDER_BUY_ENERGY_AMOUNT_TOO_SMALL": "order amount too small. Cannot less than 40000",
    "CANNOT_SET_PARTIAL_FULFILLED": "order amount less than 100000 cannot set partial fulfilled",
    "INTERNAL_ACCOUNT_NOT_FOUND": "internal account not exists",
    "ORDER_BUY_ENERGY_CAN_NOT_CREATE": "Something error occurs when create order, cannot create, please try later",
    "INTERNAL_BALANCE_ACCOUNT_TOO_LOW": "Balance is not enough"
}
```
{% endtab %}

{% tab title="401: Unauthorized" %}
```json
{
    "API_KEY_REQUIRED": "Missing api key in headers",
    "INVALID_API_KEY": "api key not correct"
}
```
{% endtab %}

{% tab title="429: Too Many Requests" %}
```json
{
    "RATE_LIMIT": "Rate limit reached"
}
```
{% endtab %}
{% endtabs %}

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://api.tronsave.io/v0/internal-buy-energy' \
  --header 'apikey: YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "resource_type": "ENERGY",
    "amount": 40000,
    "buy_energy_type": "MEDIUM",
    "duration_millisec": 3600000,
    "target_address": "YOUR_TRON_ADDRESS",
    "allow_partial_fill": false,
    "only_create_when_fulfilled": false,
    "max_price_accepted": 100,
    "add_order_incomplete": false
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const buyEnergy = async () => {
  const url = "https://api.tronsave.io/v0/internal-buy-energy";
  const body = {
    resource_type: "ENERGY",
    buy_energy_type: "MEDIUM", // price in SUN, or "SLOW" | "MEDIUM" | "FAST"
    amount: 100000, // amount of resource to buy
    allow_partial_fill: true,
    target_address: "YOUR_TRON_ADDRESS",
    duration_millisec: 86400000, // order duration in ms. Default: 259200000 (3 days)
    only_create_when_fulfilled: false,
    max_price_accepted: 100,
    add_order_incomplete: false,
  };
  const res = await fetch(url, {
    method: "POST",
    headers: {
      apikey: "YOUR_API_KEY",
      "content-type": "application/json",
    },
    body: JSON.stringify(body),
  });
  return res.json();
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v0/internal-buy-energy"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "resource_type": "ENERGY",
    "buy_energy_type": "MEDIUM",  # price in SUN, or "SLOW" | "MEDIUM" | "FAST"
    "amount": 100000,
    "allow_partial_fill": True,
    "target_address": "YOUR_TRON_ADDRESS",
    "duration_millisec": 86400000,
    "only_create_when_fulfilled": False,
    "max_price_accepted": 100,
    "add_order_incomplete": False,
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

public class BuyEnergy {
    public static void main(String[] args) throws Exception {
        String url = "https://api.tronsave.io/v0/internal-buy-energy";
        String body = """
            {
              "resource_type": "ENERGY",
              "buy_energy_type": "MEDIUM",
              "amount": 100000,
              "allow_partial_fill": true,
              "target_address": "YOUR_TRON_ADDRESS",
              "duration_millisec": 86400000,
              "only_create_when_fulfilled": false,
              "max_price_accepted": 100,
              "add_order_incomplete": false
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
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
	url := "https://api.tronsave.io/v0/internal-buy-energy"
	body := []byte(`{
		"resource_type": "ENERGY",
		"buy_energy_type": "MEDIUM",
		"amount": 100000,
		"allow_partial_fill": true,
		"target_address": "YOUR_TRON_ADDRESS",
		"duration_millisec": 86400000,
		"only_create_when_fulfilled": false,
		"max_price_accepted": 100,
		"add_order_incomplete": false
	}`)

	req, _ := http.NewRequest("POST", url, bytes.NewBuffer(body))
	req.Header.Set("apikey", "YOUR_API_KEY")
	req.Header.Set("Content-Type", "application/json")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	out, _ := io.ReadAll(resp.Body)
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "https://api.tronsave.io/v0/internal-buy-energy";
    let body = json!({
        "resource_type": "ENERGY",
        "buy_energy_type": "MEDIUM",
        "amount": 100000,
        "allow_partial_fill": true,
        "target_address": "YOUR_TRON_ADDRESS",
        "duration_millisec": 86400000_u64,
        "only_create_when_fulfilled": false,
        "max_price_accepted": 100,
        "add_order_incomplete": false
    });

    let client = reqwest::blocking::Client::new();
    let response = client
        .post(url)
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

***

## Get One Order Details

Get the details of one order by API key.

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v0/orders/:id`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

### Headers

<table><thead><tr><th width="150">Name</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that represents your internal account.</td></tr></tbody></table>

<sub>\* Required.</sub>

### Response

```json
{
    "id": "string",                 // id of order
    "requester": "string",          // the address representing the order owner
    "target": "string",             // the address that receives the resource
    "resource_amount": "number",    // the amount of resource
    "resource_type": "string",      // the resource type is "ENERGY"
    "remain_amount": "number",      // the remaining amount the system can match
    "price": "number",              // price unit is SUN
    "duration": "number",           // rent duration, in seconds
    "allow_partial_fill": "boolean",// allow the order to be filled partially or not
    "payout_amount": "number",      // total payout of this order
    "fulfilled_percent": "number",  // fill progress, 0-100
    "matched_delegates": [          // all matched delegates for this order
       {
         "delegator": "string",     // the address that delegates resource to target address
         "amount": "number",        // the amount of resource delegated
         "txid": "string"           // the on-chain transaction id
       }
    ]
}
```

```json
{
      "id": "651d2306e55c073f6ca0992e",
      "requester": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
      "target": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
      "resource_amount": 100000,
      "resource_type": "ENERGY",
      "remain_amount": 0,
      "price": 67.5,
      "duration": 3600,
      "allow_partial_fill": true,
      "payout_amount": 6750000,
      "fulfilled_percent": 100,
      "matched_delegates": [
          {
              "delegator": "TKVSaJQDWeKFSEXmA44pjxduGTxy888888",
              "amount": 100000,
              "txid": "transaction_id_1"
          }
      ]
}
```

{% hint style="info" %}
The canonical path for this endpoint is `GET https://api.tronsave.io/v0/order/:id`, where `:id` is the order id. Pass the id as a path segment (not a query parameter). All examples below use this path.
{% endhint %}

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://api.tronsave.io/v0/order/ORDER_ID' \
  --header 'apikey: YOUR_API_KEY'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const getOneOrderDetails = async (orderId) => {
  const url = `https://api.tronsave.io/v0/order/${orderId}`;
  const res = await fetch(url, {
    headers: { apikey: "YOUR_API_KEY" },
  });
  return res.json();
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

order_id = "ORDER_ID"
url = f"https://api.tronsave.io/v0/order/{order_id}"
headers = {"apikey": "YOUR_API_KEY"}

response = requests.get(url, headers=headers)
print(response.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetOneOrderDetails {
    public static void main(String[] args) throws Exception {
        String orderId = "ORDER_ID";
        String url = "https://api.tronsave.io/v0/order/" + orderId;

        HttpClient client = HttpClient.newHttpClient();
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
	orderID := "ORDER_ID"
	url := "https://api.tronsave.io/v0/order/" + orderID

	req, _ := http.NewRequest("GET", url, nil)
	req.Header.Set("apikey", "YOUR_API_KEY")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	out, _ := io.ReadAll(resp.Body)
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    let order_id = "ORDER_ID";
    let url = format!("https://api.tronsave.io/v0/order/{order_id}");

    let client = reqwest::blocking::Client::new();
    let response = client
        .get(&url)
        .header("apikey", "YOUR_API_KEY")
        .send()?;

    println!("{}", response.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

***

## Get Internal Account Order History

Get many orders sorted by creation time. Default: returns the 10 newest orders.

<mark style="color:blue;">**`GET`**</mark> **`https://api.tronsave.io/v0/orders`**

{% hint style="info" %}
Rate limit: **15** requests per **1** second.
{% endhint %}

### Headers

<table><thead><tr><th width="150">Name</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that represents your internal account.</td></tr></tbody></table>

<sub>\* Required.</sub>

### Query parameters

<table><thead><tr><th width="192">Name</th><th width="182">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>page</code></td><td>Integer</td><td>Starts from 0. Default: 0.</td></tr><tr><td><code>pageSize</code></td><td>Integer</td><td>Default: 10.</td></tr></tbody></table>

### Response

```json
{
   "data": [
     {
       "id": "string",                 // id of order
       "requester": "string",          // the address representing the order owner
       "target": "string",             // the address that receives the resource
       "resource_amount": "number",    // the amount of resource
       "resource_type": "string",      // the resource type is "ENERGY"
       "remain_amount": "number",      // the remaining amount the system can match
       "price": "number",              // price unit is SUN
       "duration": "number",           // rent duration, in seconds
       "allow_partial_fill": "boolean",// allow the order to be filled partially or not
       "payout_amount": "number",      // total payout of this order
       "fulfilled_percent": "number",  // fill progress, 0-100
       "matched_delegates": [          // all matched delegates for this order
         {
           "delegator": "string",      // the address that delegates resource to target address
           "amount": "number",         // the amount of resource delegated
           "txid": "string"            // the on-chain transaction id
         }
       ]
     }
   ],
   "total": "number"
}
```

```json
{
    "data": [
        {
            "id": "651d0e5d8248d002ea08a231",
            "requester": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
            "target": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
            "resource_amount": 100000,
            "resource_type": "ENERGY",
            "remain_amount": 100000,
            "price": 45,
            "duration": 259200,
            "allow_partial_fill": false,
            "payout_amount": 4500000,
            "fulfilled_percent": 100,
            "matched_delegates": [
                {
                    "delegator": "TKVSaJQDWeKFSEXmA44pjxduGTxy888888",
                    "amount": 100000,
                    "txid": "transaction_id_1"
                }
            ]
        },
        {
            "id": "651d2306e55c073f6ca0992e",
            "requester": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
            "target": "TKVSaJQDWeKFSEXmA44pjxduGTxy999999",
            "resource_amount": 100000,
            "resource_type": "ENERGY",
            "remain_amount": 0,
            "price": 67.5,
            "duration": 3600,
            "allow_partial_fill": true,
            "payout_amount": 6750000,
            "fulfilled_percent": 100,
            "matched_delegates": [
                {
                    "delegator": "TKVSaJQDWeKFSEXmA44pjxduGTxy888888",
                    "amount": 100000,
                    "txid": "transaction_id_2"
                }
            ]
        }
    ],
    "total": 2
}
```

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://api.tronsave.io/v0/orders' \
  --header 'apikey: YOUR_API_KEY'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const getOrderHistory = async () => {
  const url = "https://api.tronsave.io/v0/orders";
  const res = await fetch(url, {
    headers: { apikey: "YOUR_API_KEY" },
  });
  return res.json();
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v0/orders"
headers = {"apikey": "YOUR_API_KEY"}
params = {"page": 0, "pageSize": 10}

response = requests.get(url, headers=headers, params=params)
print(response.json())
```
{% endtab %}

{% tab title="Java" %}
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GetOrderHistory {
    public static void main(String[] args) throws Exception {
        String url = "https://api.tronsave.io/v0/orders?page=0&pageSize=10";

        HttpClient client = HttpClient.newHttpClient();
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
	url := "https://api.tronsave.io/v0/orders?page=0&pageSize=10"

	req, _ := http.NewRequest("GET", url, nil)
	req.Header.Set("apikey", "YOUR_API_KEY")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	out, _ := io.ReadAll(resp.Body)
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::blocking::Client::new();
    let response = client
        .get("https://api.tronsave.io/v0/orders")
        .header("apikey", "YOUR_API_KEY")
        .query(&[("page", "0"), ("pageSize", "10")])
        .send()?;

    println!("{}", response.text()?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

***

## Activate Wallet Address

Before activating any wallet address, you must first check its current activation status. Only addresses with status **0** (Inactive) should be submitted for activation.

### Step 1 — Check Active Status

Check the activation status of one or more TRON wallet addresses.

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/helper/is-active-address-check`**

#### Request body

<table><thead><tr><th width="140">Field</th><th width="150">Type</th><th width="110">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>addresses</code></td><td>array&#x3C;string></td><td>true</td><td>List of TRON wallet addresses to check.</td></tr></tbody></table>

```json
{
  "addresses": [
    "address1",
    "address2",
    "address3"
  ]
}
```

#### Response

Returns an integer array where each value corresponds to the address at the same index in the request.

<table><thead><tr><th width="100">Value</th><th width="150">Status</th><th>Description</th><th>Action</th></tr></thead><tbody><tr><td>0</td><td>Inactive</td><td>The address exists but has never been activated.</td><td>Proceed to Step 2 to activate.</td></tr><tr><td>1</td><td>Contract</td><td>The address is a smart contract.</td><td>Skip — activation is not applicable.</td></tr><tr><td>2</td><td>Active</td><td>The address is already activated.</td><td>Skip — no action needed.</td></tr><tr><td>3</td><td>Fetch failed</td><td>Could not retrieve the status from the network.</td><td>Retry later or check connectivity.</td></tr><tr><td>4</td><td>Invalid address</td><td>The address format is not valid.</td><td>Verify and correct the address.</td></tr></tbody></table>

{% hint style="info" %}
Filter the result array and collect all addresses where the corresponding status value equals `0`. Those addresses are used in **Step 2**.
{% endhint %}

#### Example

{% tabs %}
{% tab title="Body" %}
```json
{
  "addresses": [
    "TLRkWWsDDikzXxGKpVXpEzRH8bpCoL2222",
    "TZ2RPVoKVoqxAkhhTUycni8t42N2dBssss",
    "TYUdv83jM61ZQctjEXeiQNgTCXSebqRiii"
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
[
    0,
    2,
    1
]
```
{% endtab %}
{% endtabs %}

### Step 2 — Activate Wallets

Submit a batch of **inactive** addresses for activation. This endpoint creates activation requests for each address provided.

{% hint style="info" %}
**Fee:** Each activation costs **1.5 TRX per address**. Ensure your account has a sufficient balance before calling this endpoint.
{% endhint %}

<mark style="color:orange;">**`POST`**</mark> **`https://api.tronsave.io/v0/helper/multi-active-address`**

#### Headers

<table><thead><tr><th width="150">Name</th><th width="140">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>apikey</code><mark style="color:red;">*</mark></td><td>String</td><td>TronSave API key that represents your internal account.</td></tr></tbody></table>

<sub>\* Required.</sub>

#### Request body

Only include addresses with status `0` from Step 1.

<table><thead><tr><th width="130">Field</th><th width="130">Type</th><th width="106">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>addresses</code></td><td>array&#x3C;string></td><td>true</td><td>List of not-active TRON addresses to activate.</td></tr></tbody></table>

```json
{
  "addresses": [
    "address1",
    "address2",
    "address3"
  ]
}
```

#### Response

<table><thead><tr><th width="121">Field</th><th width="129">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>message</code></td><td>string</td><td>Confirmation message indicating how many activation requests were created.</td></tr></tbody></table>

```json
{
  "message": "Success create 3 active address request(s)"
}
```

#### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://api.tronsave.io/v0/helper/multi-active-address' \
  --header 'apikey: YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "addresses": [
      "TLRkWWsDDikzXxGKpVXpEzRH8bpCoL2222",
      "TZ2RPVoKVoqxAkhhTUycni8t42N2dBssss",
      "TYUdv83jM61ZQctjEXeiQNgTCXSebqRiii"
    ]
  }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const activateAddresses = async () => {
  const url = "https://api.tronsave.io/v0/helper/multi-active-address";
  const body = {
    addresses: [
      "TLRkWWsDDikzXxGKpVXpEzRH8bpCoL2222",
      "TZ2RPVoKVoqxAkhhTUycni8t42N2dBssss",
      "TYUdv83jM61ZQctjEXeiQNgTCXSebqRiii",
    ],
  };
  const res = await fetch(url, {
    method: "POST",
    headers: {
      apikey: "YOUR_API_KEY",
      "content-type": "application/json",
    },
    body: JSON.stringify(body),
  });
  return res.json();
};
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.tronsave.io/v0/helper/multi-active-address"
headers = {
    "apikey": "YOUR_API_KEY",
    "Content-Type": "application/json",
}
body = {
    "addresses": [
        "TLRkWWsDDikzXxGKpVXpEzRH8bpCoL2222",
        "TZ2RPVoKVoqxAkhhTUycni8t42N2dBssss",
        "TYUdv83jM61ZQctjEXeiQNgTCXSebqRiii",
    ]
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

public class ActivateAddresses {
    public static void main(String[] args) throws Exception {
        String url = "https://api.tronsave.io/v0/helper/multi-active-address";
        String body = """
            {
              "addresses": [
                "TLRkWWsDDikzXxGKpVXpEzRH8bpCoL2222",
                "TZ2RPVoKVoqxAkhhTUycni8t42N2dBssss",
                "TYUdv83jM61ZQctjEXeiQNgTCXSebqRiii"
              ]
            }
            """;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
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
	url := "https://api.tronsave.io/v0/helper/multi-active-address"
	body := []byte(`{
		"addresses": [
			"TLRkWWsDDikzXxGKpVXpEzRH8bpCoL2222",
			"TZ2RPVoKVoqxAkhhTUycni8t42N2dBssss",
			"TYUdv83jM61ZQctjEXeiQNgTCXSebqRiii"
		]
	}`)

	req, _ := http.NewRequest("POST", url, bytes.NewBuffer(body))
	req.Header.Set("apikey", "YOUR_API_KEY")
	req.Header.Set("Content-Type", "application/json")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	out, _ := io.ReadAll(resp.Body)
	fmt.Println(string(out))
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
use serde_json::json;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "https://api.tronsave.io/v0/helper/multi-active-address";
    let body = json!({
        "addresses": [
            "TLRkWWsDDikzXxGKpVXpEzRH8bpCoL2222",
            "TZ2RPVoKVoqxAkhhTUycni8t42N2dBssss",
            "TYUdv83jM61ZQctjEXeiQNgTCXSebqRiii"
        ]
    });

    let client = reqwest::blocking::Client::new();
    let response = client
        .post(url)
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

* Migrate to the current API: [Buy with API Key](../../api-reference/buy-resources/api-key/).
* Learn about [Authentication](../../authentication.md) and how to get an API key.
* Review [Order Types](../../../concepts/order-types.md) before placing orders.
