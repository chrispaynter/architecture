# Azure API Management

## Azure API Management

> When an API is called using a subscription key, you can access specific information about the subscription via [context.Subscription](https://docs.microsoft.com/en-us/azure/api-management/api-management-policy-expressions#ref-context-subscription) in your policy expressions. The `context.Subscription.Id` would be the ideal choice to leverage considering it would be unique for each subscription. You would however require a mapping between this ID and any ID you may have for your own system to identity customers.
>
> You could also use the [context.User](https://docs.microsoft.com/en-us/azure/api-management/api-management-policy-expressions#ref-context-user) properties if subscriptions to your API are tied to a user account.
>
> Alternatively, if you'd prefer, you could [protect your APIs using OAuth 2.0](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-protect-backend-with-aad). This way you could extract customer details after [validating the JWT](https://docs.microsoft.com/en-us/azure/api-management/api-management-access-restriction-policies#ValidateJWT)and pass the same to your backend (or the token itself if required).

https://docs.microsoft.com/en-us/answers/questions/45763/is-it-possible-to-pass-subscription-specific-param.html