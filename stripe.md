# Stripe Integration

Vatstack works seamlessly with your Stripe account. Note that integrations require a subscription plan **Launch** or above.

Once your accounts are connected, a paid invoice on Stripe will automatically create a [supply object](https://vatstack.com/docs/supplies) on Vatstack and an [evidence object](https://vatstack.com/docs/evidences) attached to it to prove the place of supply. Each invoice line item will create its own set of objects to accommodate for potentially varying VAT rates.

As the entire process is heavily automated with complex business logic, we recommend to monitor incoming transactions for accuracy in the beginning and let us know if you experience any inconsistencies.

## Determining the Tax Rate

Generally speaking, tax rates are determined with the best knowledge available. We therefore recommend that you hydrate Stripe’s invoices with as much data as possible beforehand. This includes attaching

- a tax rate applied to the invoice or to the invoice line item and
- a tax ID of your customer for potential reverse charge (read more below).

Vatstack will take this data into consideration when determining the applicable VAT rate, and whether the charged amount is inclusive or exclusive of VAT.

If none of this information is found in an invoice, we assume the VAT-inclusive standard rate of the place of supply. You would then remit VAT from the amount you have charged your customer. If a wrong tax rate was found, it is also overwritten with the applicable standard rate to ensure accurate tax declarations.

## Establishing the Place of Supply

An [evidence object](https://vatstack.com/docs/evidences) is also created simultaneously with a supply using indications found in the Stripe [invoice](https://stripe.com/docs/api/invoices) and [charge](https://stripe.com/docs/api/charges). The place of supply is established primarily by the most supporting evidences. If no majority could be determined, the following fields are used and in this priority:

- `billing_address.country_code`: Obtained from the customer’s address or shipping address.
- `bank_address.country_code`: Obtained from the Stripe charge.
- `ip_address.country_code`: Obtained from metadata (see below).

## Customer’s Tax ID

If a tax ID was found for the Stripe customer, a [validation object](https://vatstack.com/docs/validations) is also created on top and attached to the supply object. A valid tax ID will result in reverse charge for your B2B supply in jurisdictions where such a mechanism exists.

In case the tax ID is invalid or expired, we cannot apply the reverse charge mechanism and will apply the VAT rate of the place of supply. It is then considered a B2C supply and you’ll find it in your VAT MOSS report.

To save you from consuming duplicate validation hits, we query your validation records of the past 28 days prior to creating a new validation object. This follows a similar logic as suggested for a [custom implementation for re-validations](https://vatstack.com/articles/how-to-automate-vat-number-checks-before-invoice-charges).

## Customer’s IP Address

We recommend to collect your customer’s IP address during checkout as location evidence. While Stripe does collect the IP address in its checkout session, it does not reveal it through the API. We have therefore no access to it programmatically and a workaround needs to be established.

Vatstack attempts to find the IP address from these metadata fields and in this priority:

- `ip_address` field in the charge metadata.
- `ip_address` field in the customer metadata.

## Credit Notes for Refunds

When refunding a customer, we recommend to issue credit notes through Stripe. This is the only way to associate invoice line items directly with Vatstack’s supply objects. To issue a credit note, find the invoice by navigating to Customers > Invoices in your Stripe dashboard.

You can also edit the `amount_refunded` field in the supply object. The amount needs to be an absolute integer expressed in cents.
