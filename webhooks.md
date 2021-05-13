# Webhook Events

The data sent to your endpoint contains the event object which you can further process on your end. To add or edit an endpoint where the data should be sent to, navigate to **Webhooks** in your dashboard and click the **Change** button.

In the modal dialog that appears, enter a URL that accepts HTTP POST requests and returns a status code 2XX (200-299). Additional query parameters appended to the URL are allowed. Also activate the events which you would like to receive at that endpoint.

## Event Payload

The POST request sent to your endpoint generally contains the event name and its payload. Here is an example for a `validation.succeeded` event:

```
{
  "name": "validation.succeeded",
  "payload": {
    "id": "5d1ded3128ca7a842aaf5ed4",
    "active": true,
    "company_address": "3RD FLOOR, GORDON HOUSE, BARROW STREET, DUBLIN 4",
    "company_name": "GOOGLE IRELAND LIMITED",
    "company_type": null,
    "consultation_number": "WAPIAAAAW21qsOHW",
    "country_code": "IE",
    "query": "IE6388047V",
    "type": "eu_vat",
    "valid": true,
    "valid_format": true,
    "vat_number": "6388047V",
    "requested": "2019-07-04T00:00:00.000Z",
    "created": "2019-07-04T12:12:33.322Z",
    "updated": "2019-07-04T12:12:33.322Z"
  },
  "created": "2019-07-04T12:12:33.322Z",
  "updated": "2019-07-04T12:12:33.322Z"
}
```

## Event Authorization

Requests always contain an authorization header to prevent fraudulent requests from being processed on your server. The header includes a `X-API-KEY` key with your secret key.

```
"X-API-KEY": "sk_live_c283fd6d793076603646b197c7cb0424"
```

## List of Webhook Events

| Name | Description |
| --- | --- |
| `batch.failed` | Batch process could not be completed due to an internal error. |
| `batch.succeeded` | All queries in the batch process were successfully checked. |
| `validation.failed` | Response from VIES could not be obtained even after several attempts. |
| `validation.succeeded` | Received response from VIES. Check whether the VAT number is valid or not using the `valid` field. |
