# Hits

Retrieve the number of hits you have used for the running month and the total capacity allocated to your current subscription plan.

## The Hit Object

| Key | Description |
| --- | --- |
| `available` | Hits remaining for the running month. |
| `capacity` | Total capacity for the plan you’re subscribed to. |
| `used` | Hits consumed during the running month. |

## Retrieve Hits

### Request

This endpoint can be useful if you want to perform a quick check against your hits count before initiating a VAT number validation request.

```
curl -X GET https://api.vatstack.com/v1/hits \
     -H "Authorization: Credential 8637070eccf71b29f0e859f1bd5d9257" \
```

### Response

```
{
  "available": 19,
  "capacity": 100,
  "used": 81
}
```
