# Hits

Retrieve the number of hits you have used for the running month and the total capacity allocated to your current subscription plan.

A hit is consumed for every successful request. For example, we count a [validation](https://vatstack.com/docs/validations) request if it did not run into an error. Invalid number formats **do not** count as a hit. This has changed from our previous version where we still counted every API request.

The consumption is counted similarly with a [supply](https://vatstack.com/docs/supplies) request. Only successful object creations are counted. This also allows you to update a supply object without consuming an additional hit. Deleting an object will not reduce the consumed hits.

## The Hit Object

| Key | Description |
| --- | --- |
| `supplies.available` <small>deprecated</small> | Supplies remaining for the running month. |
| `supplies.capacity` | Included supplies in the plan you’re subscribed to. |
| `supplies.used` | Supplies created during the running month. Additional requests beyond `capacity` will be charged according to the plan you’re subscribed to. |
| `validations.available` <small>deprecated</small> | Validations remaining for the running month. |
| `validations.capacity` | Included validations in the plan you’re subscribed to. |
| `validations.used` | Validations created during the running month. Additional requests beyond `capacity` will be charged according to the plan you’re subscribed to. |

## Retrieve Hits

### Request

Authorize with <span class="badge badge-success">Public Key</span> <span class="badge badge-warning">Secret Key</span>

This endpoint can be useful if you want to perform a quick check against your hits count before initiating an API request.

```
curl -X GET https://api.vatstack.com/v1/hits \
     -H "X-API-KEY: pk_live_6c46e7d65bc2caccdbf48f4a9c2fcba7" \
```

### Response

Hits successfully retrieved.

```
{
  "supplies": {
    "capacity": 100,
    "used": 81
  },
  "validations": {
    "capacity": 500,
    "used": 122
  }
}
```
