# Authentication
The Vatstack API requires an API key to authenticate requests. API requests to any of the endpoints described here will fail without authentication.
You can view and manage your unique API key in the [dashboard](https://dashboard.vatstack.com/). There are several ways to authenticate requests with your unique API key.

## API Key in Query Parameters

Add your API key to every request by including `key` in the query params. When retrieving a list of your validations from the past, for example, your GET request could look like this:

```
https://api.vatstack.com/v1/validations?key=8637070eccf71b29f0e859f1bd5d9257
```

You can see that the request URI quickly becomes illegible. You can alternatively supply your API key in your headers. See below for instructions.

## API Key in Headers

In your request’s headers, add a string to the `Authorization` key that containing the `Credential` method and your API key separated with a space. Example for your authorization header using a sample API key:

```
Authorization: 'Credential 8637070eccf71b29f0e859f1bd5d9257'
```

This method can be convenient to globally declare an authorization header for your entire app. Supplying your API key in each query params will become redundant.
