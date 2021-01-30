# Authentication

The Vatstack API requires a <span class="badge badge-success">Public Key</span> or a <span class="badge badge-warning">Secret Key</span> to authenticate requests. API requests to any of the endpoints described here will fail without authentication.

You can view and manage your [unique API keys in the dashboard](https://dashboard.vatstack.com/keys).

## Public Key or Secret Key in Headers

Pass your API key into the `X-API-KEY` header. Example for your request using a sample public key:

```
"X-API-KEY": "pk_6c46e7d65bc2caccdbf48f4a9c2fcba7"
```

Some endpoints require a secret key to protect your data from being spoofed or from being manipulated. Our documentation will always mention which of the keys you can use to perform a request.
