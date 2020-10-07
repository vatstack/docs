# Webhook Events

The data sent to your endpoint contains the event object which you can further process on your end. To add or edit an endpoint where the data should be sent to, navigate to **Webhooks** in your dashboard and click the **Change** button.

In the modal dialog that appears, enter a URL that accepts HTTP POST requests and returns a status code 2XX (200-299). Additional query parameters appended to the URL are allowed. Also activate the events which you would like to receive at that endpoint.

## Event Payload

The POST request sent to your endpoint generally contains the event name and its payload. Here is an example for a `validation.succeeded` event:

```
{
  "name": "validation.succeeded",
  "payload": {
    "id": "5d9f548b5b407ab2b9d12623",
    "code": "MS_UNAVAILABLE",
    "company_address": null,
    "company_name": null,
    "consultation_number": null,
    "country_code": "IE",
    "query": "IE6388047V",
    "type": "eu_vat",
    "valid": null,
    "valid_format": true,
    "vat_number": null,
    "requested": "2019-10-10T15:55:55.660Z",
    "created": "2019-10-10T15:55:55.661Z",
    "updated": "2019-10-10T15:55:55.661Z"
  },
  "created": "2019-10-10T15:55:55.661Z",
  "updated": "2019-10-10T15:55:55.661Z"
}
```

## Event Authorization

Requests always contain an authorization header to prevent fraudulent requests from being processed on your server. The authorization header is a string containing the `Credential` method and your secret key.

```
Authorization: 'Credential sk_c283fd6d793076603646b197c7cb0424'
```

## List of Webhook Events

| Name | Description |
| --- | --- |
| `batch.failed` | Batch process could not be completed due to an internal error. |
| `batch.succeeded` | All queries in the batch process were successfully checked. |
| `validation.failed` | Response from VIES could not be obtained even after several attempts. |
| `validation.succeeded` | Received response from VIES. Check whether the VAT number is valid or not using the `valid` field. |
