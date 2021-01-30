# Batches

Batch mass VAT number validations for due dates. Add VAT numbers in bulk or append VAT numbers incrementally to your existing batch process, then start it whenever required.

You can use batches to perform VAT number validations in bulk. It’s also possible to [run a batch from your dashboard](https://vatstack.com/articles/vat-number-mass-validation-in-bulk). Once you start the validation process, Vatstack will automatically create [validation objects](https://vatstack.com/docs/validations) for each query you had added to the batch. Batches can be used and maintained flexibly according to your business requirements:

### Start the validation process immediately:

- ​Create a new batch object (POST request) by providing the `name` and an array of `queries` in the request body, and take note of the unique identifier `id` obtained from the response.
- Change the batch’s `status` to `scheduled` (PUT request) using the `id` obtained in the previous step. This will start the validation process by putting the batch on a queue.

### Start the validation process in the future:

- Create a new batch object (POST request) by providing only a `name` in the request body, and take note of the unique identifier `id` obtained in the response. You can also query a list of pending batches later on if you didn’t take note of the new `id`.
- Incrementally append VAT numbers (PUT request) that you want to validate on a deadline. For example you can append the VAT numbers of new users.
- Once you have collected all VAT numbers which you want to batch process, change the batch’s `status` to `scheduled` (PUT request) using the `id` obtained in the first step. This will start the validation process by putting the batch on a queue.

### Batch VAT validations using the dashboard:

You don’t have to use the API if you don’t want to implement batches programmatically. The [dashboard](https://dashboard.vatstack.com/) lets you upload CSV files conveniently and start the validation process with an easy-to-use interface. All validation results can be downloaded as CSV anytime.

## The Batch Object

| Key | Description |
| --- | --- |
| `id` | Unique identifier for the object. |
| `created` | ISO date at which the object was created. |
| `name` | Descriptive name of the object. |
| `queries` | Array of all queries in the order of your input. |
| `queries_ignored` | Array of all queries which are of invalid format. Ignored queries are automatically identified upon your input. Its items will not be validated. |
| `scheduled` | ISO date at which the object status was changed to `scheduled`. Defaults to `null` while the status is `pending`. |
| `status` | Status of the validation process. Batches are initialized with the `pending` status. You can change the status to `scheduled` upon which the validation process is scheduled for processing. Once picked up, the status changes to `processing`. In rare cases, the status can change to `error` which requires user amendments. If all validations succeeded, the status finally changes to `completed`. |
| `succeeded_count` | The number of `validations` where `valid` is `true` or `false`. It’s an indicator for how far the batch has progressed. |
| `updated` | ISO date at which the object was updated. |
| `validations` | Array of the first 20 validation objects in the order of `queries`. If an `id` is shown in the object, it was created for the batch and you can retrieve it individually. See [validation object](https://vatstack.com/docs/validations) for reference. |

## Create a Batch

Creates a batch object.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X POST https://api.vatstack.com/v1/batches \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Body Parameters

| Parameter | Description |
| --- | --- |
| `name` <small>required</small> | Descriptive name of the batch object. Give it any name you like to serve for your reference. |
| `queries` <small>optional</small> | Array of VAT numbers to be queried. Should be an array of strings. |

The `queries` array can be left empty if you merely want to create a new batch object and to obtain its unique identifier `id` for manipulation later on.

Batches will not process until you explicitly change their `status` field to `scheduled`. Use a PUT request to change a status. This gives you the opportunity to append `queries` over time and to start the batch validation process once a deadline is due.

### Response

Batch object successfully created.

```
{
  "id": "5dae0611273f2a47db1acd27",
  "name": "Validations for November",
  "queries": [],
  "queries_ignored": [],
  "scheduled": null,
  "status": "pending",
  "succeeded_count": 0,
  "validations": {
    "has_more": false,
    "validations": [],
    "validations_count": 0
  },
  "created": "2019-10-21T19:25:05.501Z",
  "updated": "2019-10-21T19:25:05.501Z"
}
```

## List All Batches

Retrieves all batch objects in order of creation, with the latest appearing highest.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/batches \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Query Parameters

| Parameter | Description |
| --- | --- |
| `limit` <small>optional</small> | A limit on the number of object to be returned. Limit can be 1 to 100, and the default is 20. |
| `page` <small>optional</small> | Integer for the current page. |
| `status` <small>optional</small> | Show only objects with the given `status`. |

### Response

Batch objects successfully retrieved. Example includes one valid query and one invalid query.

```
{
  "has_more": false,
  "batches_count": 5,
  "batches": [
    {
      "id": "5dae0611273f2a47db1acd27",
      "name": "Validations for November",
      "queries": [
        "DE811128135",
        "INVALIDNUMBER"
      ],
      "queries_count": 2,
      "queries_ignored": [
        "INVALIDNUMBER"
      ],
      "queries_ignored_count": 1,
      "scheduled": null,
      "status": "pending",
      "succeeded_count": 1,
      "created": "2019-10-21T19:25:05.501Z",
      "updated": "2019-10-21T19:43:12.348Z"
    },
    ...
  ]
}
```

## Retrieve a Batch

Retrieves a batch object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X GET https://api.vatstack.com/v1/batches/:id \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Response

Batch object successfully retrieved.

```
{
  "id": "5dae0611273f2a47db1acd27",
  "name": "Validations for November",
  "queries": [
    "DE811128135",
    "INVALIDNUMBER"
  ],
  "queries_count": 2,
  "queries_ignored": [
    "INVALIDNUMBER"
  ],
  "queries_ignored_count": 1,
  "scheduled": null,
  "status": "pending",
  "succeeded_count": 1,
  "validations": {
    "has_more": false,
    "validations": [
      {
        "query": "DE811128135",
        "valid": null,
        "valid_format": true
      },
      ...
    ],
    "validations_count": 2
  },
  "created": "2019-10-21T19:25:05.501Z",
  "updated": "2019-10-21T19:43:12.348Z"
}
```

## Update a Batch

Updates a batch object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X PUT https://api.vatstack.com/v1/batches/:id \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Body Parameters

| Parameter | Description |
| --- | --- |
| `mode` <small>optional</small> | Either append to `append` an array of queries to the existing array of queries, or `replace` to empty the existing array of queries prior to adding. Defaults to `append`. |
| `name` <small>optional</small> | Descriptive name of the batch object. |
| `queries` <small>optional</small> | Array of VAT numbers to be added while considering the `mode`. |
| `status` <small>optional</small> | Set it to `scheduled` once you want to schedule the validation process with the queries provided. It’s possible to add queries and change the status in one go. |

This method is especially useful if you want to gradually append new VAT numbers over time, for example, towards an upcoming submission deadline for your VAT MOSS return. Simply add a `queries` array to your request body to add new VAT numbers to the batch.

Duplicates will be purged and invalid VAT numbers ignored automatically for you. You don’t have to check whether or not a VAT number already exists in the batch.

Once you are ready to start the validation process, change the `status` to `scheduled`. Depending on the availability of various government services, this process can take several hours. It is recommended that you start the batch several days prior to a deadline. In most cases, all validations in a batch should succeed immediately or within a few hours.

### Response

Batch object successfully updated.

```
{
  "id": "5dae0611273f2a47db1acd27",
  "name": "Validations for November",
  "queries": [
    "DE811128135",
    "INVALIDNUMBER"
  ],
  "queries_count": 2,
  "queries_ignored": [
    "INVALIDNUMBER"
  ],
  "queries_ignored_count": 1,
  "scheduled": "2019-10-22T12:24:42.325Z",
  "status": "scheduled",
  "succeeded_count": 1,
  "validations": {
    "has_more": false,
    "validations": [
      {
        "query": "DE811128135",
        "valid": null,
        "valid_format": true
      },
      ...
    ],
    "validations_count": 2
  },
  "created": "2019-10-21T19:25:05.501Z",
  "updated": "2019-10-22T12:24:42.843Z"
}
```

## Delete a Batch

Deletes a batch object by the **:id** path parameter.

### Request

Authorize with <span class="badge badge-warning">Secret Key</span>

```
curl -X DELETE https://api.vatstack.com/v1/batches/:id \
     -H "X-API-KEY: sk_c283fd6d793076603646b197c7cb0424" \
```

### Response

Batch object successfully deleted.

```
{
  "id": "5dae0611273f2a47db1acd27",
  "deleted": true
}
```

## Webhook Events

Vatstack can proactively notify your server as soon as a batch was processed. This means that you don’t have to query our API anymore and can instead listen to webhook events.

See our [webhooks documentation](https://vatstack.com/docs/webhooks) for more information.

| Event | Description |
| --- | --- |
| `batch.failed` | Batch process could not be completed due to an internal error. |
| `batch.succeeded` | All queries in the batch process were successfully checked. |
