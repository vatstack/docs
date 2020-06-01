# Rates

Charge the correct VAT rate by querying the customer’s country code. This ensures that your invoice states the correct final amount as VAT rates vary from European member state to member state.

We recommend to use the [quotes object](https://vatstack.com/docs/quotes) to determine the prices you should display to your customer. It has been designed to adapt VAT dynamically according to your and your customer’s locations.

## The Rate Object

Aside from retrieving VAT rates applicable to your customer, you can also obtain net and gross prices to display during checkout. The API will do the calculation for you. Simply attach the `price` to your query to present the correct total price to your customer.

| Key | Description |
| --- | --- |
| `abbreviation` | Abbreviation of `local_name`. |
| `categories` | Categories relevant for digital products. Supports `audiobook`, `ebook` and `eperiodical`. Default to `standard_rate` if your digital product category is not available or contact us. |
| `country_code` | 2-letter ISO country code. |
| `country_name` | Corresponding English name of `country_code`. |
| `currency` | 3-letter ISO 4217 local currency code. |
| `ip_address` | The same IP address coming from the `ip_address` query params, or the geolocated IP address if none was provided. |
| `local_name` | Localized name of the VAT identification number. |
| `member_state` | Boolean indicating whether the country is an EU member state. |
| `price.gross` | Gross amount in cents, after applying the applicable VAT rate. |
| `price.net` | Net amount in cents, as requested in your query. |
| `price.vat` | VAT amount in cents. |
| `price.vat_rate` | VAT rate applied for the calculation. This can equal the `standard_rate` or the rate of the category as per your request. If `member_state` is `false`, this value will be `0`. |
| `reduced_rates` | Array of reduced VAT rates in percent. |
| `standard_rate` | Standard VAT rate in percent. |
| `vat_abbreviation` | Abbreviation of `vat_local_name`. |
| `vat_local_name` | Localized name of the VAT. |

## List All Rates

Retrieves all VAT rate objects, including standard VAT rates and VAT rates for digital products.

You can filter the results by a 2-letter ISO country code to only obtain the rate object for a specific member state. This method requires that you request a customer’s country code yourself.

### Request

```
curl -X GET https://api.vatstack.com/v1/rates \
     -H "Authorization: Credential 8637070eccf71b29f0e859f1bd5d9257" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `limit` <small>optional</small> | A limit on the number of object to be returned. Limit can be 1 to 100, and the default is 20. |
| `page` <small>optional</small> | Integer for the current page. |
| `country_code` <small>optional</small> | Filter results by a 2-letter ISO country code of the EU member state. If found, the returned response will only list the rate object for that country code. |
| `price` <small>optional</small> | Net price **in cents** (e.g. 100.50 must be expressed as 10050). This common workaround prevents unexpected rounding issues. Applies to all objects returned. |
| `category` <small>optional</small> | If the `price` is provided, you can provide the digital products category relevant for calculation. Supports `audiobook`, `ebook` and `eperiodical`. Applies to all objects returned. |

### Response

VAT rates successfully obtained with price object for 100 EUR.

```
{
  "has_more": false,
  "rates": [
    {
      "abbreviation": "UID",
      "categories": {
        "audiobook": 20,
        "ebook": 20,
        "eperiodical": 20
      },
      "country_code": "AT",
      "country_name": "Austria",
      "currency": "EUR",
      "local_name": "Umsatzsteuer-Identifikationsnummer",
      "member_state": true,
      "price": {
        "vat": 2000,
        "vat_rate": 20,
        "net": 10000,
        "gross": 12000
      },
      "reduced_rates": [
        10,
        13
      ],
      "standard_rate": 20,
      "vat_abbreviation": "MwSt.",
      "vat_local_name": "Mehrwertsteuer"
    },
    ...
  ],
  "rates_count": 28
}
```

## Retrieve Geolocated Rates

Retrieves a geolocated VAT rate by IP address.

While you can let customers choose the country they reside in, it may result in fraudulent data. It is best practice to geolocate the customer with their IP address, as the IP address counts as your tax evidence for VAT MOSS returns.

You can leave out the IP address in your query and Vatstack will automatically use the IP address coming from the request. Leaving out the IP address can be useful if the request originates from a front-end application.

### Request

```
curl -X GET https://api.vatstack.com/v1/rates/geolocate \
     -H "Authorization: Credential 8637070eccf71b29f0e859f1bd5d9257" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `ip_address` <small>optional</small> | IPv4 or IPv6 address to geolocate. If none is provided, the IP address of the request is used. |
| `price` <small>optional</small> | Net price **in cents** (e.g. 100.50 must be expressed as 10050). This common workaround prevents unexpected rounding issues. |
| `category` <small>optional</small> | If the `price` is provided, you can provide the digital products category relevant for calculation. Supports `audiobook`, `ebook` and `eperiodical`. |

### Response

VAT rate successfully obtained.

```
{
  "abbreviation": "USt-IdNr.",
  "categories": {
    "audiobook": 7,
    "ebook": 19,
    "eperiodical": 19
  },
  "country_code": "DE",
  "country_name": "Germany",
  "currency": "EUR",
  "ip_address": "81.169.181.179",
  "local_name": "Umsatzsteuer-Identifikationsnummer",
  "member_state": true,
  "price": {
    "gross": 11900,
    "net": 10000,
    "vat": 1900,
    "vat_rate": 19
  },
  "reduced_rates": [
    7
  ],
  "standard_rate": 19,
  "vat_abbreviation": "MwSt.",
  "vat_local_name": "Mehrwertsteuer"
}
```
