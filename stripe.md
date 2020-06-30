# Stripe Integration

Vatstack works seamlessly with your Stripe account. Note that integrations require a subscription plan **Small Business** or above.

Once your accounts are connected, a new invoice on Stripe will automatically create a [supply object](https://vatstack.com/docs/supplies) on Vatstack and an [evidence object](https://vatstack.com/docs/evidences) attached to it to prove the place of supply.

## Place of Supply

The place of supply is determined with the help of indications found in the Stripe’s invoice. Since an evidence object is created simultaneously with the a supply, we use these fields from it and in this priority:

- `bank_address.country_code`: Obtained from the Stripe charge.
- `ip_address.country_code`: Obtained from metadata (see below).
- `billing_address.country_code`: Obtained from the customer’s address or shipping address.

## Customer’s Tax ID

If a tax ID was found for the Stripe customer, a [validation object](https://vatstack.com/docs/validations) is also created on top and attached to the supply object. A valid tax ID will result in reverse charge for your B2B supply in jurisdictions where such a mechanism exists.

In case the tax ID is invalid or has expired, we cannot apply the reverse charge mechanism and will apply the VAT rate of the place of supply.

## Customer’s IP Address

We recommend to collect your customer’s IP address during checkout as location evidence. While Stripe does collect the IP address, it does not reveal it through the API. Vatstack has therefore no access to it and a workaround needs to be established. Vatstack attempts to find the IP address from these fields and in this priority:

- `ip_address` field in the charge metadata.
- `ip_address` field in the customer metadata.
