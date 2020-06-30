# Supplies

Supplies are transactions of telecommunications, broadcasting and electronic (TBE) services. To document pieces of evidence for the place of supply, you can attach [evidence objects](https://vatstack.com/docs/evidences) to a supply object. Supplies are also used to automatically generate a quarterly tax report with currency conversion at official exchange rates.

If, for example, your supplies are transacted in USD but your reporting currency is in EUR, we will convert each amount accurately at the applicable [exchange rate published by the European Central Bank](https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/index.en.html). We currently support domestic VAT, VAT MOSS report and EC Sales List (ESL).

### Evidence Sufficiency

We help you ensure that the evidences attached to your supply object are sufficient. The `evidence_status` indicates whether they are `sufficient` or `insufficient` to prove the place of supply as stated in `country_code`. The status depends on the number of evidences you have configured in your tax treatment settings.

### Synchronize With Integrations

You can create supply objects programmatically via this API endpoint or via integrations such as [Stripe](https://vatstack.com/docs/stripe).

Integrations with other popular payment providers follow in due course and according to demand. Please write to [team@vatstack.com](mailto:team@vatstack.com) if you require a particular integration. We’re open to discussing your needs.

## The Supply Object

| Key | Description |
| --- | --- |
| `id` | Unique identifier for the object. |
| `amount` | Amount in cents, as given in your request. |
| `amount_refunded` | Amount in cents refunded back to the customer. |
| `amount_total` | Total amount in cents charged in consideration of the `vat.inclusive` boolean. This is the amount that was actually charged the customer and is the difference between `amount` and `amount_refunded`. |
| `country_code` | 2-letter ISO country code of the place of supply that is relevant for the `vat.rate`. |
| `currency` | 3-letter ISO 4217 currency code used to charge the `amount`. |
| `created` | ISO date at which the object was created. |
| `description` | An arbitrary string to describe the supplied item. Often useful for displaying to users. |
| `evidence` | Populated evidence object if an ID is attached. You can attach an evidence object with the `evidence` body parameter in the POST request. Defaults to `null`. See [evidence object](https://vatstack.com/docs/evidences) for reference. |
| `evidence_status` | Status of whether the attached evidence object sufficiently proves the place of supply established in `country_code`. Will be either `sufficient` or `insufficient`. |
| `invoice_number` | A custom string for the invoice number issued to the customer. It’s advisable to follow sequential numbering. |
| `name` | A custom string for the name of the customer. |
| `notes` | A custom string for additional notes. |
| `updated` | ISO date at which the object was updated. |
| `validation` | Populated validation object if an ID is attached. You can attach a validation object with the `validation` body parameter in the POST request. Defaults to `null`. See [validation object](https://vatstack.com/docs/validations) for reference. |
| `vat.amount` | VAT amount in cents. |
| `vat.inclusive` | Specifies if the given `amount_total` is inclusive (common for EU consumers) or exclusive of VAT. This affects how the `vat.amount` is calculated. |
| `vat.rate` | VAT rate applicable for the place of supply established in `country_code`. |
| `vat.rate_type` | Automatically determined type of VAT rate based on inputs. Can be `null`, `exempt`, `reduced`, `reverse_charge`, `standard` or `zero`. |

## Create a Supply

Creates a supply object.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X POST https://api.vatstack.com/v1/supplies \
     -H "Authorization: Credential sk_c283fd6d793076603646b197c7cb0424" \
```

### Body Parameters

| Parameter | Description |
| --- | --- |
| `amount` <small>required</small> | Amount **in cents** (e.g. 100.50 must be expressed as 10050). This common common workaround prevents unexpected rounding issues. |
| `amount_refunded` <small>optional</small> | Amount in cents refunded back to the customer. |
| `country_code` <small>required</small> | 2-letter ISO country code of the place of supply that is relevant for the `vat.rate`. |
| `currency` <small>required</small> | 3-letter ISO 4217 currency code used to charge the `amount`. The currency is used to correctly convert to your reporting currency. |
| `description` <small>optional</small> | A custom string to describe the supplied item. |
| `evidence` <small>optional</small> | Unique identifier of an [evidence object](https://vatstack.com/docs/evidences). The pieces of non-contradictory evidence contained therein will affect the `evidence_status`. |
| `invoice_number` <small>required</small> | A custom string for the invoice number issued to the customer. It’s advisable to follow sequential numbering. |
| `issued` <small>required</small> | ISO date at which the invoice was issued to the customer. |
| `name` <small>optional</small> | A custom string for the name of the customer. |
| `notes` <small>optional</small> | A custom string for additional notes. |
| `validation` <small>optional</small> | Unique identifier of a [validation object](https://vatstack.com/docs/validations). This is useful if the customer had validated a VAT number beforehand. Its `valid` value can affect `vat.amount`, `vat.rate` and `amount_total` when zero-rating. |
| `vat.rate` <small>required</small> | VAT rate must be either the `standard_rate` or one of the `reduced_rates` in the [rate object](https://vatstack.com/docs/rates) of `country_code`. If an invalid VAT rate is provided, it is automatically replaced with the `standard_rate`. The recommended way is to use the VAT rate determined by a previously generated [quotes object](https://vatstack.com/docs/quotes) during checkout. |

### Response

Supply object successfully created.

```
{
  "amount": 9900,
  "amount_refunded": 0,
  "amount_total": 9900,
  "country_code": "IE",
  "created": "2020-06-04T21:53:37.326Z",
  "currency": "USD",
  "description": null,
  "evidence": {
    "bank_address": {
      "name": "VISA",
      "country_code": "IE"
    },
    "billing_address": {
      "city": "East Wall",
      "state": "Leinster",
      "country_code": "IE"
    },
    "ip_address": {
      "label": "92.251.255.11",
      "provider": "MaxMind GeoIP2",
      "city": "East Wall",
      "state": "Leinster",
      "country_code": "IE"
    },
    "required_count": 1,
    "created": "2020-06-04T21:53:21.698Z",
    "updated": "2020-06-04T21:53:21.698Z",
    "id": "5ed96d51336f006f30a50462"
  },
  "evidence_status": "sufficient",
  "id": "5ed96d61336f006f30a50463",
  "invoice_number": "310B7863-0007",
  "issued": "2020-06-04T20:43:17.582Z",
  "name": "John Casper",
  "notes": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
  "updated": "2020-06-04T21:53:37.326Z",
  "validation": null,
  "vat": {
    "inclusive": true,
    "rate_type": "standard",
    "rate": 23,
    "amount": 1851
  }
}
```

## List All Supplies

Retrieves all supply objects in order of creation, with the most recent appearing highest.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/supplies \
     -H "Authorization: Credential sk_c283fd6d793076603646b197c7cb0424" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `country_code` <small>optional</small> | The 2-letter ISO country code by which you want to filter your records. |
| `issued_since` <small>optional</small> | Show only objects where the `issued` date is this date or later. Format `YYYY-MM-DD`. |
| `issued_until` <small>optional</small> | Show only objects where the `issued` date is this date or earlier. Format `YYYY-MM-DD`. |
| `limit` <small>optional</small> | A limit on the number of objects to be returned. Limit can range between 1 and 100, and the default is 20. |
| `name` <small>optional</small> | Show only objects with that customer name. |
| `page` <small>optional</small> | Integer for the current page. |

### Response

Supply objects successfully retrieved.

```
{
  "has_more": false,
  "supplies_count": 1,
  "supplies": [
    {
      "amount": 9900,
      "amount_refunded": 0,
      "amount_total": 9900,
      "country_code": "IE",
      "created": "2020-06-04T21:53:37.326Z",
      "currency": "USD",
      "description": null,
      "evidence": {
        "bank_address": {
          "name": "VISA",
          "country_code": "IE"
        },
        "billing_address": {
          "city": "East Wall",
          "state": "Leinster",
          "country_code": "IE"
        },
        "ip_address": {
          "label": "92.251.255.11",
          "provider": "MaxMind GeoIP2",
          "city": "East Wall",
          "state": "Leinster",
          "country_code": "IE"
        },
        "required_count": 1,
        "created": "2020-06-04T21:53:21.698Z",
        "updated": "2020-06-04T21:53:21.698Z",
        "id": "5ed96d51336f006f30a50462"
      },
      "evidence_status": "sufficient",
      "id": "5ed96d61336f006f30a50463",
      "invoice_number": "310B7863-0007",
      "issued": "2020-06-04T20:43:17.582Z",
      "name": "John Casper",
      "notes": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
      "updated": "2020-06-04T21:53:37.326Z",
      "validation": null,
      "vat": {
        "inclusive": true,
        "rate_type": "standard",
        "rate": 23,
        "amount": 1851
      }
    },
    ...
  ]
}
```

## Retrieve a Supply

Retrieves a supply object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/supplies/:id \
     -H "Authorization: Credential sk_c283fd6d793076603646b197c7cb0424" \
```

### Response

Supply object successfully retrieved.

```
{
  "amount": 9900,
  "amount_refunded": 0,
  "amount_total": 9900,
  "country_code": "IE",
  "created": "2020-06-04T21:53:37.326Z",
  "currency": "USD",
  "description": null,
  "evidence": {
    "bank_address": {
      "name": "VISA",
      "country_code": "IE"
    },
    "billing_address": {
      "city": "East Wall",
      "state": "Leinster",
      "country_code": "IE"
    },
    "ip_address": {
      "label": "92.251.255.11",
      "provider": "MaxMind GeoIP2",
      "city": "East Wall",
      "state": "Leinster",
      "country_code": "IE"
    },
    "required_count": 1,
    "created": "2020-06-04T21:53:21.698Z",
    "updated": "2020-06-04T21:53:21.698Z",
    "id": "5ed96d51336f006f30a50462"
  },
  "evidence_status": "sufficient",
  "id": "5ed96d61336f006f30a50463",
  "invoice_number": "310B7863-0007",
  "issued": "2020-06-04T20:43:17.582Z",
  "name": "John Casper",
  "notes": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
  "updated": "2020-06-04T21:53:37.326Z",
  "validation": null,
  "vat": {
    "inclusive": true,
    "rate_type": "standard",
    "rate": 23,
    "amount": 1851
  }
}
```

## Update a Supply

Updates a supply object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X PUT https://api.vatstack.com/v1/supplies/:id \
     -H "Authorization: Credential sk_c283fd6d793076603646b197c7cb0424" \
```

### Body Parameters

| Parameter | Description |
| --- | --- |
| `amount` <small>required</small> | Amount **in cents** (e.g. 100.50 must be expressed as 10050). This common common workaround prevents unexpected rounding issues. |
| `amount_refunded` <small>optional</small> | Amount in cents refunded back to the customer. |
| `country_code` <small>required</small> | 2-letter ISO country code of the place of supply that is relevant for the `vat.rate`. |
| `currency` <small>required</small> | 3-letter ISO 4217 currency code used to charge the `amount`. The currency is used to correctly convert to your reporting currency. |
| `description` <small>optional</small> | A custom string to describe the supplied item. |
| `evidence` <small>optional</small> | Unique identifier of an [evidence object](https://vatstack.com/docs/evidences). The pieces of non-contradictory evidence contained therein will affect the `evidence_status`. |
| `invoice_number` <small>required</small> | A custom field for the invoice number issued to the customer. It’s advisable to follow sequential numbering. |
| `issued` <small>required</small> | ISO date at which the invoice was issued to the customer. |
| `name` <small>optional</small> | A custom field for the name of the customer. |
| `notes` <small>optional</small> | A custom field for additional notes. |
| `validation` <small>optional</small> | Unique identifier of a [validation object](https://vatstack.com/docs/validations). This is useful if the customer had validated a VAT number beforehand. Its `valid` value can affect `vat.amount`, `vat.rate` and `amount_total` when zero-rating. |
| `vat.rate` <small>required</small> | VAT rate must be either the `standard_rate` or one of the `reduced_rates` in the [rate object](https://vatstack.com/docs/rates) of `country_code`. If an invalid VAT rate is provided, it is automatically replaced with the `standard_rate`. The recommended way is to use the VAT rate determined by a previously generated [quotes object](https://vatstack.com/docs/quotes) during checkout. |

### Response

Supply object successfully updated.

```
{
  "amount": 9900,
  "amount_refunded": 0,
  "amount_total": 9900,
  "country_code": "IE",
  "created": "2020-06-04T21:53:37.326Z",
  "currency": "USD",
  "description": null,
  "evidence": {
    "bank_address": {
      "name": "VISA",
      "country_code": "IE"
    },
    "billing_address": {
      "city": "East Wall",
      "state": "Leinster",
      "country_code": "IE"
    },
    "ip_address": {
      "label": "92.251.255.11",
      "provider": "MaxMind GeoIP2",
      "city": "East Wall",
      "state": "Leinster",
      "country_code": "IE"
    },
    "required_count": 1,
    "created": "2020-06-04T21:53:21.698Z",
    "updated": "2020-06-04T21:53:21.698Z",
    "id": "5ed96d51336f006f30a50462"
  },
  "evidence_status": "sufficient",
  "id": "5ed96d61336f006f30a50463",
  "invoice_number": "310B7863-0007",
  "issued": "2020-06-04T20:43:17.582Z",
  "name": "John Casper",
  "notes": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
  "updated": "2020-06-04T22:12:54.512Z",
  "validation": null,
  "vat": {
    "inclusive": true,
    "rate_type": "standard",
    "rate": 23,
    "amount": 1851
  }
}
```

## Delete a Supply

Deletes a supply object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X DELETE https://api.vatstack.com/v1/supplies/:id \
     -H "Authorization: Credential sk_c283fd6d793076603646b197c7cb0424" \
```

### Response

Supply object successfully deleted.

```
{
  "id": "5ed96d61336f006f30a50463",
  "deleted": true
}
```
