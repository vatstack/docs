# Getting Started

Implement validation requests within minutes using the code samples for various programming environments outlined below.

Note that the API key in each code block must be replaced with the unique API access key found in your dashboard. You can find more details in the [authentication](https://vatstack.com/docs/authentication) reference.

To obtain validation results, you send a POST request to our API endpoint. Use GET requests to retrieve historical validations you have performed in the past.

## Node.js

We will use [Axios](https://github.com/axios/axios) in order to perform requests from the Node backend. Please refer to their documentation for installation instructions.

```
const axios = require('axios')
​
var api_key = '8637070eccf71b29f0e859f1bd5d9257'
var vat_id = 'IE6388047V'
​
// Create a validation request via HTTP POST.
axios.post('https://api.vatstack.com/v1/validations', {
  vat_id: vat_id
}, {
  headers: { Authorization: `Credential ${ api_key }` }
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
$api_key = '8637070eccf71b29f0e859f1bd5d9257';
$vat_id = 'IE6388047V';
​
// Initialize CURL.
$ch = curl_init('https://api.vatstack.com/v1/validations');
​
curl_setopt($ch, CURLOPT_FAILONERROR, true);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
​
// Add VAT number to body.
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, 'vat_id=' . $vat_id);
​
// Add key to header array.
curl_setopt($ch, CURLOPT_HTTPHEADER, ['Authorization: Credential ' . $api_key]);
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
var api_key = '8637070eccf71b29f0e859f1bd5d9257'
var vat_id = 'IE6388047V'
​
// Perform an AJAX POST request.
$.ajax({
  url: 'https://api.vatstack.com/v1/validations',
  type: 'post',
  data: { vat_id: vat_id },
  dataType: 'json',
  headers: { Authorization: `Credential ${ api_key }` },
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
