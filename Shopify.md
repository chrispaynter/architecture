# Shopify

[TOC]

## Resources
- [REST Admin API reference](https://shopify.dev/api/admin-rest/2021-10/resources/discountcode#top)
  - The Admin API lets you build apps and integrations that extend and enhance the Shopify admin.
  - [Shopify Order Object](https://shopify.dev/api/admin-rest/2021-07/resources/order#resource_object)
- [Deconstructing the Monolith: Designing Software that Maximizes Developer Productivity](https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity)
- [Under Deconstruction: The State of Shopify’s Monolith](https://shopify.engineering/shopify-monolith)

## Shopify are architecting a Modular Monolith

- [Source](https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity)

## Shopify checkouts are converted to Orders

- Checkouts are converted to orders once the order is complete. There is no webhook for checkout/complete as that would essentially be the same as order/creation. [Source](https://community.shopify.com/c/shopify-apis-and-sdks/what-exactly-is-the-difference-between-order-and-checkout/td-p/193982)

## Checkouts are marked as abandoned immediately

- It's worth also noting that an abandoned checkout is created the moment a customer hits the checkout and it will remain in that state until completed or recovered. [Source](https://community.shopify.com/c/shopify-apis-and-sdks/checkouts-vs-orders/td-p/451615)

## Use of DDD

> For our main monolith, we took the first approach; our vision was guided by the ideas of Domain Driven Design. We defined components as implementations of subdomains of the domain of commerce, and moved the files into corresponding folders. 
>
> [Source](https://webcache.googleusercontent.com/search?q=cache:aPQh2seIjakJ:https://shopify.engineering/shopify-monolith+&cd=1&hl=en&ct=clnk&gl=fi)

## Shopify try to avoid you creating custom checkouts

- Two types of apps
  - Private
    - Your own custom application
  - Public
    - Facebook
    - Google
- Private apps are not supposed to be used to customsie checkout
  - They want you to user their own web urls for this

> As per our documentation ([https://shopify.dev/docs/storefront-api/reference/mutation/checkoutcompletewithcreditcardv2?api[vers...](https://shopify.dev/docs/storefront-api/reference/mutation/checkoutcompletewithcreditcardv2?api[version]=2020-04)), the Storefront API mutations of 'CheckoutCompleteWithCreditCardV2' requires your app to be approved by Shopify to handle payment processing, and in order to be approved by Shopify for this your app must first be a public Sales Channel that serves multiple merchants. These are the requirements. If your app doesn't meet this, then you should utilize the 'webUrl' field to complete the payment processing through the Shopify Checkout - that is the most secure, safest, and efficient option.
>
> https://community.shopify.com/c/storefront-api-and-sdks/convert-existing-private-app-into-sales-channel/td-p/746708

