# Authentication

The Vatstack API requires a <span class="badge badge-success">Public Key</span> or a <span class="badge badge-warning">Secret Key</span> to authenticate requests. API requests to any of the endpoints described here will fail without authentication.

You can view and manage your [unique API keys in the dashboard](https://dashboard.vatstack.com/keys). There are two ways with which you can authenticate requests using the public key.

## Public Key in Query Parameters

Add your public key to every request by including `key` in the query params. When retrieving a list of your validations from the past, for example, your GET request could look like this:

```
https://api.vatstack.com/v1/validations?key=pk_6c46e7d65bc2caccdbf48f4a9c2fcba7
```

You can see that the request URI quickly becomes illegible. You can alternatively supply your public key in your headers. See below for instructions.

## Public Key or Secret Key in Headers

Pass your API key into the `X-API-KEY` header. Example for your request using a sample public key:

```
"X-API-KEY": "pk_6c46e7d65bc2caccdbf48f4a9c2fcba7"
```

This method can be convenient to globally declare an authorization header for your entire app. Supplying your API key in each query params will become redundant.

Some endpoints require a secret key to protect your data from spoofing or from being manipulated. Our documentation will always mention which of the keys you can use to perform a request.
