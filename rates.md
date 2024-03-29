# Rates

Keep the rates of your application up-to-date with a regular VAT rate lookup. This ensures that your invoice states the correct VAT across EU Member States. We have also started to add countries outside the EU and will keep them updated.

VAT rates can change irregularly as governments adapt to new circumstances. We track such announcements closely and store scheduled changes in our database. Any such change will take effect at exactly midnight local time of the respective country.

If you want to quote VAT prices to your customer during checkout, we recommend to use the [quotes object](https://vatstack.com/docs/quotes). It’s a powerful endpoint because it has been designed to adapt VAT dynamically according to your and your customer’s locations.

## The Rate Object

| Key | Description |
| --- | --- |
| `abbreviation` | Abbreviation of `local_name`. |
| `categories` | Categories relevant for digital products. Object contains VAT specific to `audiobook`, `broadcasting`, `ebook`, `eperiodical`, `eservice` and `telecommunication`. |
| `country_code` | 2-letter ISO country code. |
| `country_name` | Corresponding English name of `country_code`. |
| `currency` | 3-letter ISO 4217 local currency code. |
| `local_name` | Localized name of the VAT identification number. |
| `member_state` | Boolean indicating whether the country is an EU Member State. |
| `reduced_rates` | Array of reduced VAT rates in percent. |
| `standard_rate` | Standard VAT rate in percent. |
| `state_code` | 2-letter US state code. Only required if `country_code` is `US`. |
| `state_name` | Corresponding English name of `state_code`. |
| `vat_abbreviation` | Abbreviation of `vat_local_name`. |
| `vat_local_name` | Localized name of the VAT. |

## List All Rates

Retrieves all VAT rate objects, including standard VAT rates and VAT rates for digital products.

You can filter the results by a 2-letter ISO country code to only obtain the rate object for a specific country.

### Request

Authorize with <span class="badge badge-success">Public Key</span> <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/rates \
     -H "X-API-KEY: pk_live_6c46e7d65bc2caccdbf48f4a9c2fcba7" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `country_code` <small>optional</small> | Filter results by a 2-letter ISO country code. |
| `limit` <small>optional</small> | A limit on the number of object to be returned. Limit can be 1 to 100, and the default is 20. |
| `member_state` <small>optional</small> | Boolean to filter results by EU Member State. Always `true` without a subscription. |
| `page` <small>optional</small> | Integer for the current page. |
| `state_code` <small>optional</small> | Filter results by a 2-letter US state code. Required if `country_code` is `US`. |

### Response

VAT rates successfully obtained for `member_state` set to `true`.

```
{
  "has_more": false,
  "rates_count": 27,
  "rates": [
    {
      "abbreviation": "UID",
      "categories": {
        "audiobook": 10,
        "broadcasting": 10,
        "ebook": 10,
        "eperiodical": 10,
        "eservice": 20,
        "telecommunication": 20
      },
      "country_code": "AT",
      "country_name": "Austria",
      "currency": "EUR",
      "local_name": "Umsatzsteuer-Identifikationsnummer",
      "member_state": true,
      "reduced_rates": [
        10,
        13
      ],
      "standard_rate": 20,
      "state_code": null,
      "state_name": null,
      "vat_abbreviation": "MwSt.",
      "vat_local_name": "Mehrwertsteuer"
    },
    ...
  ]
}
```

## Retrieve Geolocated Rates

Retrieves a geolocated VAT rate by IP address.

While you can let customers choose the country they reside in, it may result in fraudulent data. It is best practice to geolocate the customer with their IP address, as the IP address counts as your tax evidence for VAT OSS returns.

You can leave out the IP address in your query and Vatstack will automatically use the IP address coming from the request. Leaving out the IP address can be useful if the request originates from a front-end application.

### Request

Authorize with <span class="badge badge-success">Public Key</span> <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/rates/geolocate \
     -H "X-API-KEY: pk_live_6c46e7d65bc2caccdbf48f4a9c2fcba7" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `ip_address` <small>optional</small> | IPv4 or IPv6 address to geolocate. If none is provided, the IP address of the request is used. |

### Response

VAT rate successfully obtained.

```
{
  "abbreviation": "UID",
  "categories": {
    "audiobook": 10,
    "broadcasting": 10,
    "ebook": 10,
    "eperiodical": 10,
    "eservice": 20,
    "telecommunication": 20
  },
  "country_code": "AT",
  "country_name": "Austria",
  "currency": "EUR",
  "local_name": "Umsatzsteuer-Identifikationsnummer",
  "member_state": true,
  "reduced_rates": [
    10,
    13
  ],
  "standard_rate": 20,
  "state_code": null,
  "state_name": null,
  "vat_abbreviation": "MwSt.",
  "vat_local_name": "Mehrwertsteuer"
}
```
