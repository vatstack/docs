# Evidences

You must [collect two pieces of non-contradictory evidence](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex%3A32013R1042) that confirm your customer’s location and keep them on file for 10 years. Therefore measures must be put in place to capture the necessary information that will help to establish where the telecommunications, broadcasting and electronic (TBE) service has been consumed.

### VAT E-commerce Package

If you’re a business in the EU with less than 100,000 EUR in cross-border sales of TBE annually, then you only need to [collect one piece of non-contradictory customer location evidence](https://ec.europa.eu/taxation_customs/business/vat/digital-single-market-modernising-vat-cross-border-ecommerce_en).

EU businesses with less than 10,000 EUR in cross-border sales of TBE annually can opt out of VAT MOSS altogether. In such cases, you are not obliged to collect any piece of evidence. Set your VAT MOSS opt-in status in your dashboard’s [regional settings](https://dashboard.vatstack.com/regions) accordingly.

Our article about [EU VAT rules for digital businesses](https://vatstack.com/articles/eu-vat-guide-for-digital-subscription-businesses) has more information on the thresholds.

### Types of Evidences

You can store the following pieces of location evidence:

- Location of the customer’s bank
- Billing address of the customer (valid only if provided by a 3rd party)
- IP address of the device used by the customer

All evidence must be gathered from a third party, such as the bank or IP address, and not from the customer directly. After all, the customer could twist the facts to evade paying tax. Nevertheless, we let you store the customer’s full billing address but encourage you to always store the bank’s address and IP address preferentially.

The evidence object can be attached to a [supply object](https://vatstack.com/docs/supplies) and it can only be attached to one supply object at a time.

## The Evidence Object

| Key | Description |
| --- | --- |
| `id` | Unique identifier for the object. |
| `bank_address.country_code` | 2-letter ISO country code of the bank or payment source. |
| `bank_address.name` | Name of the bank or payment source. |
| `billing_address.city` | City of the customer’s billing address. |
| `billing_address.country_code` | 2-letter ISO country code of the customer’s billing address. |
| `billing_address.line_1` | Line 1 of the customer’s billing address. |
| `billing_address.line_2` | Line 2 of the customer’s billing address. |
| `billing_address.postal_code` | Postal code of the customer’s billing address. |
| `billing_address.state` | State of the customer’s billing address. |
| `created` | ISO date at which the object was created. |
| `ip_address.label` | The same IP address coming from the `ip_address.label` body parameter which will be geolocated automatically. |
| `ip_address.provider` | Provider used to geolocate `ip_address.label`. We use MaxMind&reg; GeoIP2 geolocation technology by default but have a number of fallback providers. |
| `ip_address.city` | City of the geolocated IP address. |
| `ip_address.country_code` | 2-letter ISO country code of the geolocated IP address. |
| `ip_address.state` | State of the geolocated IP address. |
| `required_count` | Required pieces of evidence according to your account’s regional settings. |
| `updated` | ISO date at which the object was updated. |

## Create an Evidence

Creates an evidence object.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X POST https://api.vatstack.com/v1/evidences \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Body Parameters

| Parameter | Description |
| --- | --- |
| `bank_address.name` <small>optional</small> | Name of the bank or payment source used by the customer. This information is usually provided by your payments provider. |
| `bank_address.country_code` <small>optional</small> | 2-letter ISO country code of the bank or payment source. This information is usually provided by your payments provider. |
| `billing_address.city` <small>optional</small> | City of the customer’s billing address. |
| `billing_address.country_code` <small>optional</small> | 2-letter ISO country code of the customer’s billing address. |
| `billing_address.line_1` <small>optional</small> | Line 1 of the customer’s billing address. |
| `billing_address.line_2` <small>optional</small> | Line 2 of the customer’s billing address. |
| `billing_address.postal_code` <small>optional</small> | Postal code of the customer’s billing address. |
| `billing_address.state` <small>optional</small> | State of the customer’s billing address. |
| `ip_address.label` <small>optional</small> | IP address of the customer at the time of supply. We will automatically geolocate this IP address and hydrate the other fields. |

### Response

Evidence object successfully created.

```
{
  "id": "5ed804577853dd1c229f938a",
  "bank_address": {
    "name": "Visa",
    "country_code": "IE"
  },
  "billing_address": {
    "line_1": null,
    "line_2": null,
    "city": "East Wall",
    "country_code": "IE"
    "postal_code": null,
    "state": "Leinster",
  },
  "created": "2020-06-03T20:13:12.437Z",
  "ip_address": {
    "label": "92.251.255.11",
    "provider": "MaxMind GeoIP2",
    "city": "East Wall",
    "state": "Leinster",
    "country_code": "IE"
  },
  "required_count": 1,
  "updated": "2020-06-03T20:13:12.437Z"
}
```

## List All Evidences

Retrieves all evidence objects in order of creation, with the most recent appearing highest.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/evidences \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `limit` <small>optional</small> | Limit on the number of objects to be returned. Limit can range between 1 and 100, and the default is 20. |
| `page` <small>optional</small> | Integer for the current page. |

### Response

Evidence objects successfully retrieved.

```
{
  "has_more": false,
  "evidences_count": 9,
  "evidences": [
    {
      "id": "5ed804577853dd1c229f938a",
      "bank_address": {
        "name": "Visa",
        "country_code": "IE"
      },
      "billing_address": {
        "line_1": null,
        "line_2": null,
        "city": "East Wall",
        "country_code": "IE"
        "postal_code": null,
        "state": "Leinster",
      },
      "created": "2020-06-03T20:13:12.437Z",
      "ip_address": {
        "label": "92.251.255.11",
        "provider": "MaxMind GeoIP2",
        "city": "East Wall",
        "state": "Leinster",
        "country_code": "IE"
      },
      "required_count": 1,
      "updated": "2020-06-03T20:13:12.437Z"
    },
    ...
  ]
}
```

## Retrieve an Evidence

Retrieves an evidence object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/evidences/:id \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Response

Evidence object successfully retrieved.

```
{
  "id": "5ed804577853dd1c229f938a",
  "bank_address": {
    "name": "Visa",
    "country_code": "IE"
  },
  "billing_address": {
    "line_1": null,
    "line_2": null,
    "city": "East Wall",
    "country_code": "IE"
    "postal_code": null,
    "state": "Leinster",
  },
  "created": "2020-06-03T20:13:12.437Z",
  "ip_address": {
    "label": "92.251.255.11",
    "provider": "MaxMind GeoIP2",
    "city": "East Wall",
    "state": "Leinster",
    "country_code": "IE"
  },
  "required_count": 1,
  "updated": "2020-06-03T20:13:12.437Z"
}
```

## Update an Evidence

Updates an evidence object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X PUT https://api.vatstack.com/v1/evidences/:id \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Body Parameters

| Parameter | Description |
| --- | --- |
| `bank_address.name` <small>optional</small> | Name of the bank or payment source used by the customer. This information is usually provided by your payments provider. |
| `bank_address.country_code` <small>optional</small> | 2-letter ISO country code of the bank or payment source. This information is usually provided by your payments provider. |
| `billing_address.city` <small>optional</small> | City of the customer’s billing address. |
| `billing_address.country_code` <small>optional</small> | 2-letter ISO country code of the customer’s billing address. |
| `billing_address.line_1` <small>optional</small> | Line 1 of the customer’s billing address. |
| `billing_address.line_2` <small>optional</small> | Line 2 of the customer’s billing address. |
| `billing_address.postal_code` <small>optional</small> | Postal code of the customer’s billing address. |
| `billing_address.state` <small>optional</small> | State of the customer’s billing address. |
| `ip_address.label` <small>optional</small> | IP address of the customer at the time of supply. We will automatically geolocate this IP address and hydrate the other fields. |

### Response

Evidence object successfully updated.

```
{
  "id": "5ed804577853dd1c229f938a",
  "bank_address": {
    "name": "Visa",
    "country_code": "IE"
  },
  "billing_address": {
    "line_1": null,
    "line_2": null,
    "city": "East Wall",
    "country_code": "IE"
    "postal_code": null,
    "state": "Leinster",
  },
  "created": "2020-06-03T20:13:12.437Z",
  "ip_address": {
    "label": "92.251.255.11",
    "provider": "MaxMind GeoIP2",
    "city": "East Wall",
    "state": "Leinster",
    "country_code": "IE"
  },
  "required_count": 1,
  "updated": "2020-06-03T21:32:54.245Z"
}
```
