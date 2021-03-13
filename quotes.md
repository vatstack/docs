# Quotes

To ensure that the correct country’s VAT rate is applied under the rules for telecommunications, broadcasting and electronic (TBE) services, it is fundamental that the place of supply is determined. Responsibility for determining the customer’s location falls to the supplier.

The quote object combines the best of the [rate object](https://vatstack.com/docs/rates) and [validation object](https://vatstack.com/docs/validations) and takes them a step further. It uses your existing business information to perform the **entire VAT-compliant business logic** for you. All VAT rules have been centralized so that every response is directly actionable.

### Intelligent Tax Treatment

Our core resources for [rates](https://vatstack.com/docs/rates) and [validations](https://vatstack.com/docs/validations) are “cold” fetches because they are provided as is and you still have to research tax regulations that specifically apply to each sale. We consider responses from the quotes endpoints as “warm” fetches because they are **dynamically adapted to the circumstances between your business and your customer**.

For example, if your business is situated in Italy and your customer is also in Italy, the quoted price will always include a VAT because VAT is always charged when transactions occur within the same EU member state (even for business customers). The reverse-charge mechanism does not apply here. This is a tax rule that you would have had to implement yourself, but is now taken care of by the API.

### Shareable Between Applications

The ideal way to use the quote object is to generate a new one at checkout. Once your visitor checks out, you can use the object’s unique identifier to charge the exact same amount that your visitor has been presented with earlier.

You don’t have to replicate the same business logic in the frontend and the backend applications anymore. You can generate as many quotes as you like. Quote objects are **cached for 3 days** and then deleted.

> **Preventing abuse**: If you generate quote objects from your frontend application, you should perform logical tests of the object’s content before you proceed with the actual charge to prevent abuse. For example, compare the object’s `amount` with your product’s price. If visitors get hold of your public key, they can create new quote objects themselves.

## The Quote Object

| Key | Description |
| --- | --- |
| `id` | Unique identifier for the object. |
| `abbreviation` | Abbreviation of `local_name`. |
| `amount` | Amount in cents, as given in your request. |
| `amount_total` | Total amount to be charged in consideration of the `vat.inclusive` boolean. This is the amount you should display to customers. |
| `category` | Category of the digital product. Defaults to `null` if no category was specified in the request or if the category cannot be applied for the `country_code`. |
| `country_code` | 2-letter ISO country code. |
| `country_name` | Corresponding English name of `country_code`. |
| `created` | ISO date at which the object was created. |
| `integrations` | Array of objects which are hydrated if you connect Vatstack with a payment processor. It can include the applicable `tax_rate_id` of your payment processor. |
| `ip_address` | The same IP address coming from the `ip_address` body parameter, or the geolocated IP address if none was provided. Value is `null` if `country_code` was provided. |
| `local_name` | Localized name of the VAT identification number. |
| `member_state` | Boolean indicating whether `country_code` is an EU member state. |
| `updated` | ISO date at which the object was updated. |
| `validation` | Populated validation object if an ID is attached. You can attach a validation object with the `validation` body parameter in the POST request. Defaults to `null`. See [validation object](https://vatstack.com/docs/validations) for reference. |
| `vat.abbreviation` | Abbreviation of `vat.local_name`. |
| `vat.amount` | VAT amount in cents. |
| `vat.inclusive` | Specifies if the `amount_total` is inclusive (common for EU consumers) or exclusive of VAT. This affects how the `vat.amount` is calculated. If `false`, you should present `amount` plus `vat.amount` to your customer as the final price to pay. |
| `vat.local_name` | Localized name of the VAT. |
| `vat.rate` | VAT rate applied for the calculation. If `member_state` is `false`, the value will be `0`. |
| `vat.rate_type` | Automatically determined type of VAT rate based on inputs. Can be `null`, `exempt`, `reduced`, `reverse_charge`, `standard` or `zero`. |

## Create a quote

Creates a quote object.

### Request

Authorize with <span class="badge badge-success">Public Key</span> <span class="badge badge-warning">Secret Key</span>

```
curl -X POST https://api.vatstack.com/v1/quotes \
     -H "X-API-KEY: pk_6c46e7d65bc2caccdbf48f4a9c2fcba7" \
```

### Body Parameters

| Parameter | Description |
| --- | --- |
| `amount` <small>required</small> | Amount **in cents** (e.g. 100.50 must be expressed as 10050). This common common workaround prevents unexpected rounding issues. |
| `country_code` <small>optional</small> | 2-letter ISO country code. If provided, the `ip_address` parameter will be ignored. |
| `category` <small>optional</small> | Digital products category used for calculation. Supports `audiobook`, `broadcasting`, `ebook`, `eperiodical`, `eservice` and `telecommunication`. |
| `ip_address` <small>optional</small> | IP address to geolocate the VAT rate for. If neither IP address nor `country_code` is provided, it will be automatically determined from the request. |
| `validation` <small>optional</small> | Unique identifier of a [validation object](https://vatstack.com/docs/validations). This is useful if you let your customer enter a VAT number beforehand. Its `valid` value can affect `vat.amount`, `vat.rate` and `amount_total` when zero-rating. |
| `vat.inclusive` <small>optional</small> | Boolean for whether the resulting VAT amount should be calculated inclusive or exclusive of VAT. Defaults to `false`. All other `vat` fields will be hydrated for you. |

### Response

Quote object successfully created. Example includes a populated [validation object](https://vatstack.com/docs/validations).

```
{
  "id": "5dc490ea73c1ce2f69628900",
  "abbreviation": "VAT ID no.",
  "amount": 10000,
  "amount_total": 10000,
  "category": null,
  "country_code": "IE",
  "country_name": "Ireland",
  "integrations": [
    {
      "provider": "stripe",
      "tax_rate_id": "txr_1ITsZmE4GGQvLhGdNCYVfm7M"
    }
  ],
  "ip_address": "92.251.255.11",
  "local_name": "Value added tax identification number",
  "member_state": true,
  "validation": {
    "id": "5dc490ea73c1ce2f69628901",
    "active": true,
    "company_address": "3RD FLOOR, GORDON HOUSE, BARROW STREET, DUBLIN 4",
    "company_name": "GOOGLE IRELAND LIMITED",
    "consultation_number": "WAPIAAAAW5H1hUQb",
    "company_type": null,
    "country_code": "IE",
    "query": "IE6388047V",
    "type": "eu_vat",
    "valid": true,
    "valid_format": true,
    "vat_number": "6388047V",
    "requested": "2019-11-07T00:00:00.000Z",
    "created": "2019-11-07T21:47:22.952Z",
    "updated": "2019-11-07T21:47:22.952Z"
  },
  "vat": {
    "abbreviation": "VAT",
    "amount": 0,
    "inclusive": false,
    "local_name": "Value Added Tax",
    "rate": 0,
    "rate_type": "reverse_charge"
  },
  "created": "2019-11-07T21:47:22.955Z",
  "updated": "2019-11-07T21:47:22.955Z"
}
```

## List All Quotes

Retrieves all quote objects in order of creation, with the most recent appearing highest.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/quotes \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `limit` <small>optional</small> | Limit on the number of objects to be returned. Limit can range between 1 and 100, and the default is 20. |
| `page` <small>optional</small> | Integer for the current page. |

### Response

Quote objects successfully retrieved.

```
{
  "has_more": true,
  "quotes_count": 23,
  "quotes": [
    {
      "id": "5dc490ea73c1ce2f69628900",
      "abbreviation": "VAT ID no.",
      "amount": 10000,
      "amount_total": 10000,
      "category": null,
      "country_code": "IE",
      "country_name": "Ireland",
      "integrations": [
        {
          "provider": "stripe",
          "tax_rate_id": "txr_1ITsZmE4GGQvLhGdNCYVfm7M"
        }
      ],
      "ip_address": "92.251.255.11",
      "local_name": "Value added tax identification number",
      "member_state": true,
      "validation": {
        "id": "5dc490ea73c1ce2f69628901",
        "active": true,
        "company_address": "3RD FLOOR, GORDON HOUSE, BARROW STREET, DUBLIN 4",
        "company_name": "GOOGLE IRELAND LIMITED",
        "company_type": null,
        "consultation_number": "WAPIAAAAW5H1hUQb",
        "country_code": "IE",
        "query": "IE6388047V",
        "type": "eu_vat",
        "valid": true,
        "valid_format": true,
        "vat_number": "6388047V",
        "requested": "2019-11-07T00:00:00.000Z",
        "created": "2019-11-07T21:47:22.952Z",
        "updated": "2019-11-07T21:47:22.952Z"
      },
      "vat": {
        "abbreviation": "VAT",
        "amount": 0,
        "inclusive": false,
        "local_name": "Value Added Tax",
        "rate": 0,
        "rate_type": "reverse_charge"
      },
      "created": "2019-11-07T21:47:22.955Z",
      "updated": "2019-11-07T21:47:22.955Z"
    },
    ...
  ]
}
```

## Retrieve a Quote

Retrieves a quote object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/quotes/:id \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Response

Quote object successfully retrieved.

```
{
  "id": "5dc490ea73c1ce2f69628900",
  "abbreviation": "VAT ID no.",
  "amount": 10000,
  "amount_total": 10000,
  "category": null,
  "country_code": "IE",
  "country_name": "Ireland",
  "integrations": [
    {
      "provider": "stripe",
      "tax_rate_id": "txr_1ITsZmE4GGQvLhGdNCYVfm7M"
    }
  ],
  "ip_address": "92.251.255.11",
  "local_name": "Value added tax identification number",
  "member_state": true,
  "validation": {
    "id": "5dc490ea73c1ce2f69628901",
    "active": true,
    "company_address": "3RD FLOOR, GORDON HOUSE, BARROW STREET, DUBLIN 4",
    "company_name": "GOOGLE IRELAND LIMITED",
    "company_type": null,
    "consultation_number": "WAPIAAAAW5H1hUQb",
    "country_code": "IE",
    "query": "IE6388047V",
    "type": "eu_vat",
    "valid": true,
    "valid_format": true,
    "vat_number": "6388047V",
    "requested": "2019-11-07T00:00:00.000Z",
    "created": "2019-11-07T21:47:22.952Z",
    "updated": "2019-11-07T21:47:22.952Z"
  },
  "vat": {
    "abbreviation": "VAT",
    "amount": 0,
    "inclusive": false,
    "local_name": "Value Added Tax",
    "rate": 0,
    "rate_type": "reverse_charge"
  },
  "created": "2019-11-07T21:47:22.955Z",
  "updated": "2019-11-07T21:47:22.955Z"
}
```
