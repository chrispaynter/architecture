# 7 - Services

## What a domain service isn't

- It's not a service like you might know of in **Service Orientanted Architecture (SOA)**
- Don't confuse it with an **Application Service**
  - **Domain Services** house business logic
  - **Application Services** do not house business logic.

## What a domain service is

- When a significant process or transformation in the domain is not a natural responsibility of an ENTITY or VALUE OBJECT, add an operation to the model as a standalone interface declared as a SERVICE.
  - Define the interface in terms of the language of the model, making sure it's name is part of the UBIQUITOUS LANGUAGE.
  - Make the SERVICE stateless.
- Under what condition would an operation not belong on an existing Entity or Value Object? Some are:
  - Perform a significant business process
  - Transform a domain object from one composition to another
  - Calculate a Value requiring input from one or more domain objects.
- A significant process can require one ore more aggregates or their composed parts as input.
- When it's "clumsy" to place the mothed on any one Entity or Value, it works out best to define a service.
- As per the idea of a "pure" domain model, which doesn't call out to out-of-process dependencies, repositiories should not be used inside Aggregates if at all possible.

## Make sure you need a service

- Don't lean too heavily toward modelling a domain concept as a Service.
  - Do so only if the circumstances fit.
- Using Services overzealously will usually result in the negative consequences of creating an **Anemic Domain Model**.
  - All domain logic resides in Services rather than mostly spread across Entity and Value Objects.
- How to recognise the need to model a service.

## Modeling a service in the domain

### Is Seperated Interface a Necessity?

- No, but
- 💡 Given that different tenants might eventually desire specialized security standards, it's possible that there could be multiple implementations. **Relevent to One Basket providers**
- 