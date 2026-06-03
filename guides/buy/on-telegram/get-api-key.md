---
description: Find, copy, and revoke your TronSave API key from the Telegram bot.
---

# Get the TronSave API Key

Your TronSave API key authenticates programmatic requests to the TronSave API. The Telegram bot exposes the key tied to your internal account so you can copy it for use with the REST API and SDKs.

## Step 1: Open the API key in User Info

In the bot, go to your **User Info** and select the **API key** button.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FPXZWe1N1M6t0HJdNz3Ah%2Fimage.png?alt=media&#x26;token=08fb213d-dc09-4657-b1be-41bcae55e79d" alt=""><figcaption></figcaption></figure>

## Step 2: Copy the API key

Tap the API key to copy it to your clipboard.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2Ft56PrV7m0pvsygYBCi0J%2Fimage.png?alt=media&#x26;token=219646b8-49cc-4253-9771-0994a11face6" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Treat your API key like a password. Anyone with it can spend the TRX balance on your internal account.
{% endhint %}

## Revoke the API key

If you need to change your API key, select **Revoke** to generate a new one.

{% hint style="info" %}
We do not recommend changing the API key. Only revoke it if necessary — revoking invalidates the old key, and any integration still using it will stop working.
{% endhint %}

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FXwhmyqb8SO0w1uBY4u0R%2Fimage.png?alt=media&#x26;token=83079f23-7029-463d-a142-91b1b9fefefc" alt=""><figcaption><p>Click "Revoke"</p></figcaption></figure>

Then confirm the action:

* Click **Confirm** to generate a new API key.
* Click **Cancel** to stop the action.

<figure><img src="https://1055070949-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh0rlDWTk05Qu3lvB5Gve%2Fuploads%2FvkrfT8wcPgy8C9787aKe%2Fimage.png?alt=media&#x26;token=e9bdaf25-de51-40a0-930d-ad19653632c2" alt=""><figcaption></figcaption></figure>

## Next steps

* [Authentication](../../../developers/authentication.md)
* [Buy resources with an API key](../../../developers/api-reference/buy-resources/api-key/README.md)
