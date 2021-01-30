# Getting Started

Implement quote requests within minutes using the code samples for various programming environments outlined below.

Note that the API key in each code block must be replaced with the unique public key found in your dashboard. You can find more details in the [authentication](https://vatstack.com/docs/authentication) reference.

To obtain quote results, you send a POST request to our API endpoint. Use GET requests to retrieve historical quotes you have performed in the past.

## Node.js

We will use [Axios](https://github.com/axios/axios) in order to perform requests from the Node backend. Please refer to their documentation for installation instructions.

```
const axios = require('axios')
​
var api_key = 'pk_6c46e7d65bc2caccdbf48f4a9c2fcba7'
var amount = 10000
​
// Create a quote request via HTTP POST.
axios.post('https://api.vatstack.com/v1/quotes', {
  amount: amount
}, {
  headers: { Authorization: `Basic ${ api_key }` }
})
.then(function(result) {
  // Do something with result.data.
  console.log(result.data)
})
.catch(function(err) {
  // Handle the error.
  console.error(err.response.data)
})
```

## PHP (cURL)

The best practice to perform API requests with PHP is with its built-in cURL.

```
$api_key = 'pk_6c46e7d65bc2caccdbf48f4a9c2fcba7';
$amount = 10000;
​
// Initialize CURL.
$ch = curl_init('https://api.vatstack.com/v1/quotes');
​
curl_setopt($ch, CURLOPT_FAILONERROR, true);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
​
// Add VAT number to body.
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, 'amount=' . $amount);
​
// Add key to header array.
curl_setopt($ch, CURLOPT_HTTPHEADER, ['Authorization: Basic ' . $api_key]);
​
$json = curl_exec($ch);
​
if ($json === false) {
  // Handle the error.
  echo 'Curl Error: ' . curl_error($ch);
} else {
  // Decode JSON response.
  $data = json_decode($json, true);
​
  // Do something with $data.
  var_dump($data);
}
​
// Close connection.
curl_close($ch);
```

## jQuery

You have to link [jQuery](https://jquery.com/) either from a CDN or a self-hosted source.

```
var api_key = 'pk_6c46e7d65bc2caccdbf48f4a9c2fcba7'
var amount = 10000
​
// Perform an AJAX POST request.
$.ajax({
  url: 'https://api.vatstack.com/v1/quotes',
  type: 'post',
  data: { amount: amount },
  dataType: 'json',
  headers: { Authorization: `Basic ${ api_key }` },
  success: function(result) {
    // Do something with result.data.
    console.log(result.data)
  },
  error: function(err) {
    // Handle the error.
    console.error(err.responseJSON)
  }
})
```
