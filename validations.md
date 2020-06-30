# Validations

Validate your business customer’s VAT number against official government servers in real-time. The API asynchronously validates VAT numbers for you if government services experience downtime.

This API endpoint validates VAT numbers intelligently despite government service downtimes and ensures compliant record-keeping for your tax office:

- Every response of type `eu_moss` and `eu_vat` includes a consultation number given by VIES. The consultation number is a unique reference identifier and is an official piece of evidence used to show to tax administrations of a member state that you have rightfully validated a given VAT number on a given date.
- Record-keeping can be especially useful for tax-filing purposes and if you are faced with an audit. It can also be a handy fallback for repeated charges with the same customers in the event that government services are temporarily unavailable.
- [​Government service downtimes](http://ec.europa.eu/taxation_customs/vies/help.html) happen regularly but will be a less blocking issue from now on. Validation requests are gracefully accepted and enter a schedule for automated re-validation. You can identify downtimes by a response’s error code `MS_UNAVAILABLE` or `SERVICE_UNAVAILABLE`. See error codes section for more information.
- Vatstack proactively notifies your server as soon as a validation request was successfully processed and a result obtained from official government servers. This means that you don’t have to query our API anymore and can instead listen to webhook events.

To help you better understand how Vatstack’s endpoint stands out against other proxied solutions, read more about [how you can make best use of this endpoint](https://vatstack.com/articles/introducing-the-new-validations-endpoint).

## The Validation Object

| Key | Description |
| --- | --- |
| `id` | Unique identifier for the object. |
| `code` | In the event of an error, this field will contain the error code. See the list of error codes below and their explanation. |
| `company_address` | Address of the company the VAT number is associated with. Servers of Germany and Spain won’t return a value for privacy reasons and will default to `null`. |
| `company_name` | Name of the company the VAT number is associated with. Servers of Germany and Spain won’t return a value for privacy reasons and will default to `null`. |
| `consultation_number` | If you save your own VAT number in your dashboard, the reply will contain a unique consultation number. The consultation number enables you to prove to a tax administration of a member state that you have checked a VAT number at the `requested` date, and obtained a validation result. |
| `country_code` | 2-letter ISO country code. Note that while Greek VAT numbers contain the `EL` country code, our response will return the ISO country code `GR`. |
| `created` | ISO date at which the object was created. |
| `query` | Your original query. |
| `requested` | ISO date at which the validation request was originally performed. This is the request date returned by VIES and does not specify a time. |
| `type` | Type of VAT number. One of `au_gst` (Australia), `eu_moss` (EU MOSS), `eu_vat` (VIES), `ch_vat` (Switzerland) or `no_vat` (Norway). |
| `updated` | ISO date at which the object was updated. |
| `valid` | Boolean indicating whether the `vat_number` is registered for VAT. If government services are down, the value will be `null` and re-checked automatically for you. |
| `valid_format` | Boolean indicating whether the VAT number contained in `query` is in a valid format. |
| `vat_number` | VAT number extracted from your query without the country code. |

## Create a Validation

Creates a validation object.

### Request

Authorize with <span class="badge badge-success">Public Key</span> <span class="badge badge-warning">Secret Key</span>

```
curl -X POST https://api.vatstack.com/v1/validations \
     -H "Authorization: Credential pk_6c46e7d65bc2caccdbf48f4a9c2fcba7" \
```

### Body Parameters

| Parameter | Description |
| --- | --- |
| `type` <small>optional</small> | Restrict validation to either `au_gst`, `ch_vat`, `eu_moss`, `eu_vat` or `no_vat`. If not provided, the type is automatically determined based on the VAT number given. |
| `query` <small>required</small> | VAT number that you want to validate. |

You may want to check `valid_format` on every request to give your customers feedback on their input. This boolean indicates whether your query was delivered in a valid format or not.

The validation result will be returned to you in the response. In the event that government services are temporarily unavailable, the API will respond with status code `202` and valid value `null`. Vatstack will retry unfulfilled validation requests asynchronously until a result has been obtained.

The `id` can be used to retrieve a validation status in future. Alternatively, you can obtain the status by checking the status in your dashboard, or listening to webhook events.

Finalized validation results are posted to you via webhooks. Unfulfilled validation requests will be abandoned if no result could be obtained after 2 days.

### Response for Status Code 201 (Created)

Validation object successfully created.

```
{
  "id": "5d1ded3128ca7a842aaf5ed4",
  "company_address": "3RD FLOOR, GORDON HOUSE, BARROW STREET, DUBLIN 4",
  "company_name": "GOOGLE IRELAND LIMITED",
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
}
```

### Response for Status Code 202 (Accepted)

Validation request was accepted and will resume asynchronously.

```
{
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
}
```

## List all validations

Retrieves all validation objects in order of creation, with the most recent appearing highest.

### Request

Authorize with <span class="badge badge-success">Public Key</span> <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/validations \
     -H "Authorization: Credential pk_6c46e7d65bc2caccdbf48f4a9c2fcba7" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `query` <small>optional</small> | The VAT number you want to search in the `query` field of your records. |
| `requested_since` <small>optional</small> | Show only objects where the `requested` date is this date or later. Format `YYYY-MM-DD`. |
| `requested_until` <small>optional</small> | Show only objects where the `requested` date is this date or earlier. Format `YYYY-MM-DD`. |
| `batch` <small>optional</small> | Show only objects that belong to a batch with the given identifier. |
| `limit` <small>optional</small> | A limit on the number of objects to be returned. Limit can range between 1 and 100, and the default is 20. |
| `page` <small>optional</small> | Integer for the current page. |

### Response

Validation objects successfully retrieved.

```
{
  "has_more": true,
  "validations_count": 55,
  "validations": [
    {
      "id": "5d1ded3128ca7a842aaf5ed4",
      "company_address": "3RD FLOOR, GORDON HOUSE, BARROW STREET, DUBLIN 4",
      "company_name": "GOOGLE IRELAND LIMITED",
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
    ...
  ]
}
```

## Retrieve a Validation

Retrieves a validation object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-success">Public Key</span> <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/validations/:id \
     -H "Authorization: Credential pk_6c46e7d65bc2caccdbf48f4a9c2fcba7" \
```

### Response

Validation object successfully retrieved.

```
{
  "id": "5d1ded3128ca7a842aaf5ed4",
  "company_address": "3RD FLOOR, GORDON HOUSE, BARROW STREET, DUBLIN 4",
  "company_name": "GOOGLE IRELAND LIMITED",
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
}
```

## VAT Number Formats

Vatstack currently validates the following types of VAT numbers in real-time:

- **Australia**: Australian Business Number (`au_gst`)
- **EU MOSS ID**: MOSS ID of non-EU businesses (`eu_moss`)
- **EU VAT ID**: VAT ID of EU businesses (`eu_vat`)
- **Norway**: Organization Number (`no_vat`)
- **Switzerland**: Business Identification Number (`ch_vat`)

If you have a specific requirement, we’re happy to integrate more government services. Below section explains how you can submit validation requests for each region.

### Australia

[Australia’s GST number format](https://abr.business.gov.au/Help/AbnFormat) is a 11 digit number formed from a 9 digit unique identifier and 2 leading check digits. The identifier is issued to all entities registered in the [Australian Business Register (ABR)](https://abr.business.gov.au/). It is therefore also known as the Australian Business Number (ABN). Example **51 824 753 556**.

Validate an ABN with Vatstack by providing 11 digits in your request. Vatstack will then automatically check against the ABR whether the business is registered for GST. Our announcement has more information about [ABN validation](https://vatstack.com/articles/australian-business-number-abn-validation).

### European Union (VIES)

The API validates both the VAT ID of EU businesses and the MOSS ID of non-EU businesses. The [VAT ID format](https://ec.europa.eu/taxation_customs/vies/faq.html#item_11) starts with the country code of the EU member state, followed by 8 to 12 digits or characters. Note that the country code is a two-letter [ISO 3166 alpha-2](https://www.iso.org/iso-3166-country-codes.html), except for Greece for which the abbreviation is ‘EL’. Example **EL999999999**. Learn more about the [benefits of validating VAT numbers with Vatstack](https://vatstack.com/articles/how-to-check-and-validate-eu-vat-numbers).

The MOSS ID format starts with ‘EU’, followed by 9 digits. The country code of non-EU businesses cannot be determined and is therefore shown as `null`. Example **EU999999999**.

### Norway

Businesses which are registered in the Value Added Tax Register are required to add the letters ‘MVA’ as a suffix to their organization number. The organization number has 9 digits. Example **999999999MVA**.

Vatstack detects a Norwegian VAT number by its suffix ‘MVA’ in your request and validates it against the [Central Coordinating Register](https://www.brreg.no/om-oss/oppgavene-vare/alle-registrene-vare/om-enhetsregisteret/). Our announcement has more details about [Norwegian VAT number validations](https://vatstack.com/articles/norway-vat-number-validation).

### Switzerland

Swiss number formats are based on the Swiss UID. It starts with ‘CHE’, followed by 9 digits, and either ends with ‘MWST’, ‘TVA’ or ‘IVA’ depending on the part of Switzerland a business is registered in. Example **CHE-123.456.789 MWST**.

Provide a VAT number with a ‘CHE’ prefix in your request, and Vatstack will automatically validate it against the official [UID Register](https://www.uid.admin.ch/Search.aspx?lang=en). Learn more about [Swiss VAT number validations](https://vatstack.com/articles/switzerland-vat-number-validation) in our announcement.

## Webhook Events

Vatstack will proactively notify your server as soon as a validation request was successfully processed and a result obtained from official government services. This means that you don’t have to query our API anymore and can instead listen to webhook events.

The payload contains the validation object which you can further process on your end. Configure at which endpoint they should be sent to in your dashboard. You could then reach out to the customer to rectify the situation or adjust your invoice for the next charge.

| Event | Description |
| --- | --- |
| `validation.succeeded` | Received response from VIES. Check whether the VAT number is valid or not using the `valid` field. |

## Error Codes

| Code | Description |
| --- | --- |
| `GLOBAL_MAX_CONCURRENT_REQ` | Maximum number of concurrent requests has been reached. Try to resubmit your request in a few moments. |
| `INVALID_INPUT` | VAT number is invalid. |
| `INVALID_REQUESTER_INFO` | Supplier VAT number is invalid. Verify that the VAT number entered in your account information is correct. |
| `INVALID_RESPONSE` | The response obtained is invalid and cannot be processed. |
| `MS_MAX_CONCURRENT_REQ` | Maximum number of concurrent requests for this member state service has been reached. Try to resubmit your request in a few moments. |
| `MS_UNAVAILABLE` | A member state service is currently unavailable. Your request has been saved and Vatstack will retry validations. |
| `SERVER_BUSY` | The validation service is currently busy. Try to resubmit your request in a few moments. |
| `SERVICE_UNAVAILABLE` | The validation service is currently unavailable. Your request has been saved and Vatstack will retry validations. |
| `TIMEOUT` | Member state service could not be reached in time. Try to resubmit your request in a few moments. |
| `VAT_BLOCKED` | VAT number has been blocked and cannot be queried. |
