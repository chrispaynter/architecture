# Shopify

- Two types of apps
  - Private
    - Your own custom application
  - Public
    - Facebook
    - Google

## Shopify try to avoid you creating custom checkouts

> As per our documentation ([https://shopify.dev/docs/storefront-api/reference/mutation/checkoutcompletewithcreditcardv2?api[vers...](https://shopify.dev/docs/storefront-api/reference/mutation/checkoutcompletewithcreditcardv2?api[version]=2020-04)), the Storefront API mutations of 'CheckoutCompleteWithCreditCardV2' requires your app to be approved by Shopify to handle payment processing, and in order to be approved by Shopify for this your app must first be a public Sales Channel that serves multiple merchants. These are the requirements. If your app doesn't meet this, then you should utilize the 'webUrl' field to complete the payment processing through the Shopify Checkout - that is the most secure, safest, and efficient option.