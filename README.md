# Introduction

A suite of VAT APIs which let you validate VAT numbers with official government services, look up VAT rates by ISO country code and calculate price quotes compliant with official VAT rules.

## Introduction to the Vatstack API

The Vatstack API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer). Our VAT API has predictable resource-oriented URLs, accepts [form-encoded](https://en.wikipedia.org/wiki/POST_(HTTP)#Use_for_submitting_web_forms) request bodies, returns [JSON-encoded](http://www.json.org/) responses, and uses standard HTTP response codes, authentication, and verbs.

Familiarize yourself with the core concepts of the JSON (JavaScript Object Notation) data format. JSON is a common, language-independent data format that provides a simple text representation of arbitrary data structures. For more information, see [json.org](http://json.org/). The JSON format ensures maximum compatibility with industry-standard web application frameworks and programming languages.

Using the POST method, you can perform requests and retrieve data using the GET method.

## API Specifications

### Unique API Keys

A unique public key and a unique secret key are assigned to you during registration. It is your gateway to interact with Vatstack’s API endpoints. Use your keys to authenticate all your requests using one of the [two documented authentication methods](https://vatstack.com/docs/authentication).

The keys can be rolled to a newly generated one from within your dashboard. This can be useful in the event that one of your keys has been compromised. Please mind that resetting a key can break your application if it is not updated there at the same time. You can choose a rolling period to transition without disruption.

### Sample API Response

All results are returned in industry-standard JSON format which can be easily read by your application. Here is a sample response from the `validations` POST endpoint:

```
{
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
}
```
