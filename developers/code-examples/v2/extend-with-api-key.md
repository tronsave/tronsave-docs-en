---
description: Full working example showing how to extend an existing TronSave order using an API Key, in JavaScript, PHP, and Python.
---

# Extend an Order with an API Key

This is a complete, runnable example for extending an existing order on TronSave using an **API Key**. It calls the v2 extend endpoints in two steps: first it estimates which delegates are extendable and how much TRX it will cost, then it submits the extend request and returns an `orderId`.

## Prerequisites

You need a TronSave API Key tied to an Internal Account. There are two ways to obtain one:

* **Option 1:** Generate the API Key on the TronSave website.
* **Option 2:** Generate the API Key on Telegram.

See [Authentication](../../authentication.md) for both methods and details on how the API Key authorizes requests against your Internal Account.

{% hint style="info" %}
The JavaScript example below uses only the built-in `fetch` API and does not require TronWeb. The original source noted a TronWeb 5.3.2 requirement for related examples; install it only if your wider integration needs signing:

```bash
npm i tronweb@5.3.2 @noble/secp256k1@1.7.1
```

Read more in the [TronWeb 5.3.2 release notes](https://tronweb.network/docu/docs/5.3.2/Release%20Note/).
{% endhint %}

## How it works

The flow uses two endpoints:

1. [`/v2/get-extendable-delegates`](../../api-reference/extend-orders/get-extendable-delegates.md) — returns the `extendData` payload, the estimated TRX cost, and whether the extension is possible.
2. [`/v2/extend-request`](../../api-reference/extend-orders/extend-request.md) — submits the `extendData` and returns an `orderId`.

Set these variables before running:

<table><thead><tr><th>Variable</th><th>Description</th></tr></thead><tbody>
<tr><td><code>API_KEY</code></td><td>Your TronSave API Key.</td></tr>
<tr><td><code>RECEIVER</code></td><td>The address that receives the resource delegate.</td></tr>
<tr><td><code>RESOURCE_TYPE</code></td><td><code>ENERGY</code> or <code>BANDWIDTH</code>. Optional; defaults to <code>ENERGY</code>.</td></tr>
<tr><td><code>extendTo</code></td><td>The time, in seconds (Unix timestamp), you want to extend the delegation to.</td></tr>
<tr><td><code>maxPriceAccepted</code></td><td>Optional. The maximum price (in SUN) you are willing to pay to extend.</td></tr>
</tbody></table>

## Full example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const API_KEY = `your-api-key`;
const TRONSAVE_API_URL = "https://api.tronsave.io"
const RECEIVER = "your-receiver-address"
const RESOURCE_TYPE = "ENERGY"

const GetEstimateExtendData = async (extendTo, maxPriceAccepted) => {
    const url = TRONSAVE_API_URL + `/v2/get-extendable-delegates`
    const body = {
        extendTo, //time in seconds you want to extend to
        receiver: RECEIVER,   //the address that receives the resource delegate
        maxPriceAccepted, //Optional. Number the maximum price you want to pay to extend
        resourceType: RESOURCE_TYPE, //ENERGY or BANDWIDTH. optional. The default is ENERGY
    }
    const data = await fetch(url, {
        method: "POST",
        headers: {
            'apikey': API_KEY,
            "content-type": "application/json",
        },
        body: JSON.stringify(body)
    })
    const response = await data.json()
    /**
     * Example response 
     * @link  
       {
            "error": false,
            "message": "Success",
            "data": {
                "extendOrderBook": [
                    {
                        "price": 784,
                        "value": 1002
                    }
                ],
                "totalDelegateAmount": 5003,
                "totalAvailableExtendAmount": 5003,
                "totalEstimateTrx": 4085783,
                "isAbleToExtend": true,
                "yourBalance": 2377366851,
                "extendData": [
                    {
                        "delegator": "TGGVrYaT8Xoos...6dmSZkohGGcouYL4",
                        "isExtend": true,
                        "extraAmount": 0,
                        "extendTo": 1745833276
                    },
                    {
                        "delegator": "TQBV7xU489Rq8Z...zBhJMdrDr51wA2",
                        "isExtend": true,
                        "extraAmount": 0,
                        "extendTo": 1745833276
                    },
                    {
                        "delegator": "TSHZv6xsYHMRCbdVh...qNozxaPPjDR6",
                        "isExtend": true,
                        "extraAmount": 0,
                        "extendTo": 1745833276
                    }
                ]
            }
        }
     */
    return response
}

/**
 * @param {number} extendTo the time in seconds you want to extend to
 * @param {boolean} maxPriceAccepted number maximum price you want to pay to extend
 * @returns 
 */
const SendExtendRequest = async (extendTo, maxPriceAccepted) => {
    const url = TRONSAVE_API_URL + `/v2/extend-request`
    // Get estimate extendable delegates
    const estimateResponse = await GetEstimateExtendData(extendTo, maxPriceAccepted)
    const extendData = estimateResponse.data?.extendData
    if (extendData && extendData.length) { 
        const body = {
            extendData: extendData,
            receiver: RECEIVER,
        }
        const data = await fetch(url, {
            method: "POST",
            headers: {
                'apikey': API_KEY,
                "content-type": "application/json",
            },
            body: JSON.stringify(body)
        })
        const response = await data.json()
        /**
         * Example response 
         {
            error: false,
            message: 'Success',
            data: { orderId: '680b5ac7b09a385fb3d582ff' }
            }
         */
        return response
    }
    return []
}

//Example run code
const ClientCode = async () => { 
    const extendTo = Math.floor(new Date().getTime() / 1000) + 3 * 86400 //Extend to 3 next days
    const maxPriceAccepted = 900
    const response = await SendExtendRequest(extendTo, maxPriceAccepted)
    console.log(response)
}


ClientCode()
```
{% endtab %}

{% tab title="PHP" %}
```php
//<?php

// Configuration
const API_KEY = 'your-api-key';
const TRONSAVE_API_URL = "https://api.tronsave.io";
const RECEIVER = "your-receiver-address";
const RESOURCE_TYPE = "ENERGY";

/**
 * Get estimate extend data
 * @param int $extendTo
 * @param int $maxPriceAccepted
 * @return array
 */
function getEstimateExtendData(int $extendTo, int $maxPriceAccepted): array {
    $url = TRONSAVE_API_URL . "/v2/get-extendable-delegates";
    $body = [
        'extendTo' => $extendTo,
        'receiver' => RECEIVER,
        'maxPriceAccepted' => $maxPriceAccepted,
        'resourceType' => RESOURCE_TYPE,
    ];

    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($body));
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        'apikey: ' . API_KEY,
        'Content-Type: application/json'
    ]);
    $response = curl_exec($ch);
    curl_close($ch);
    return json_decode($response, true);
}

/**
 * Send extend request
 * @param int $extendTo
 * @param int $maxPriceAccepted
 * @return array
 */
function sendExtendRequest(int $extendTo, int $maxPriceAccepted): array {
    $url = TRONSAVE_API_URL . "/v2/extend-request";
    $estimateResponse = getEstimateExtendData($extendTo, $maxPriceAccepted);
    $extendData = $estimateResponse['data']['extendData'] ?? [];

    if (!empty($extendData)) {
        $body = [
            'extendData' => $extendData,
            'receiver' => RECEIVER,
        ];

        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($body));
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_HTTPHEADER, [
            'apikey: ' . API_KEY,
            'Content-Type: application/json'
        ]);
        $response = curl_exec($ch);
        curl_close($ch);
        return json_decode($response, true);
    }

    return [];
}

// Example run code
try {
    $extendTo = time() + (3 * 86400); // Extend to 3 next days
    $maxPriceAccepted = 900;
    $response = sendExtendRequest($extendTo, $maxPriceAccepted);
    print_r($response);
} catch (Exception $e) {
    echo "Error: " . $e->getMessage() . "\n";
} 
```
{% endtab %}

{% tab title="Python" %}
```python
import time
import requests
from typing import Dict, Any, Optional

# Configuration
API_KEY = 'your-api-key'
TRONSAVE_API_URL = "https://api.tronsave.io"
RECEIVER = "your-receiver-address"
RESOURCE_TYPE = "ENERGY"

def get_estimate_extend_data(extend_to: int, max_price_accepted: int) -> Dict[str, Any]:
    """Get estimate extend data"""
    url = f"{TRONSAVE_API_URL}/v2/get-extendable-delegates"
    headers = {
        'apikey': API_KEY,
        'Content-Type': 'application/json'
    }
    
    body = {
        'extendTo': extend_to,
        'receiver': RECEIVER,
        'maxPriceAccepted': max_price_accepted,
        'resourceType': RESOURCE_TYPE,
    }
    
    response = requests.post(url, headers=headers, json=body)
    return response.json()

def send_extend_request(extend_to: int, max_price_accepted: int) -> Dict[str, Any]:
    """Send extend request"""
    url = f"{TRONSAVE_API_URL}/v2/extend-request"
    headers = {
        'apikey': API_KEY,
        'Content-Type': 'application/json'
    }
    
    estimate_response = get_estimate_extend_data(extend_to, max_price_accepted)
    extend_data = estimate_response.get('data', {}).get('extendData', [])

    if extend_data:
        body = {
            'extendData': extend_data,
            'receiver': RECEIVER,
        }
        
        response = requests.post(url, headers=headers, json=body)
        return response.json()

    return {}

def main() -> None:
    """Main function"""
    try:
        extend_to = int(time.time()) + (3 * 86400)  # Extend to 3 next days
        max_price_accepted = 900
        response = send_extend_request(extend_to, max_price_accepted)
        print("Response:", response)
    except Exception as e:
        print(f"Error: {str(e)}")

if __name__ == "__main__":
    main() 
```
{% endtab %}
{% endtabs %}

## Next steps

* [Get Extendable Delegates](../../api-reference/extend-orders/get-extendable-delegates.md) — full reference for step 1.
* [Submit Extend Request](../../api-reference/extend-orders/extend-request.md) — full reference for step 2.
* [Authentication](../../authentication.md) — how to obtain and use an API Key.
