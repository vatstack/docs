# Testing

Test your integration before going live. This page includes test VAT identification numbers and other information to make sure your test environment works before deploying for production. If you need any help with our API, please do not hesitate to reach out.

## Test API Keys

An API request sent with your test keys are automatically identified as a request in test mode. You can find your test keys in the dashboard if you turn off the “production” switch.

## International Test VAT identification numbers

Genuine VAT IDs can be used in test mode but responses will always show them as valid together with dummy business information. We have dedicated a few numbers that will behave differently to help you test various scenarios.

| VAT ID | Description |
| -- | -- |
| `BE0123123123` | Responds with status code 202 to simulate a government downtime. Such a request will be re-validated within a few minutes and change to a valid VAT ID. |
| `IT01231231234` | Responds with `valid` status set to `false`. |

Note that we may change the behavior at our own discretion and without prior notice. We recommend that you check back here if your test results change unexpectedly.

## Webhooks

You can add a dedicated webhooks endpoint for your test environment in the dashboard. Events will be sent to your endpoint at the point of request, but failed attempts will not be retried.
