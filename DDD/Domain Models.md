# Domain Models

## Resources

- [Domain model purity vs. domain model completeness](https://enterprisecraftsmanship.com/posts/domain-model-purity-completeness/)

## Domain model Purity vs Completeness vs Performance

### Completeness

- In a perfect world, our domain model would be **[complete](https://enterprisecraftsmanship.com/posts/domain-model-purity-completeness/)**, meaning:
  - all domain logic is contained within the model itself:
    - domain services
    - entities
    - value objects
  - domain logic doesn't leak out into other layers:
    - command handlers
    - application
    - API controllers
    - etc

### Purity

- In that same world, the domain model would also be **[pure](https://enterprisecraftsmanship.com/posts/domain-model-purity-completeness/)**, meaning:
  - The model doesn't reach out to out-of-process dependencies:
    - Databases (even if via a repository interface)
    - APIs
    - etc
  - Classes should only depend on primitive types or other domain classes (entites / value objects / services)

### Domain Logic Fragmentation

- However, the quest for a pure domain model will often lead to **[domain logic fragmentation](https://enterprisecraftsmanship.com/posts/domain-model-purity-completeness/)**, meaning:
  - Domain logic ends up residing in layers other than the domain layer.
  - The act of which "fragments" the domain logic, but keeps the domain pure by not having to rely on out-of-process dependencies.
  - This is of course causes the domain model to no longer be complete, as a by product.
    - Logic now lives outside the domain model
    - To be complete, all logic must live in the domain model

### An example

- Image a business rule is "make sure a user's email address is unique any time they change it"
- Part of this business rule is checking through each user in the database to see if any have the email addrress already.
- To do this inside the User entity itself would require injecting the databa infrastructure into it.
- As mentioned above, this would then make the domain less **pure**.
- One way around it is to give the User entity all the information it needs, by passing in an array of every user in the database so that it can perform the business rule itself.
  - This would also keep the domain model **complete**.
  - However, this would be achieved at to the detriment of system **performance**.
  - You can't bring potentially thousands of entries back from the database each time you want to do some domain logic.
  
- The only way around this is to fragment the domain logic
  - The rule gets  hoisted up into the application layer.
  - In the example above, we'd have to perform the business rule "users must have a unique email" in the application layer, before telling the User entity to change it's email address.

### Summary

- A domain object can be considered to be a Domain Service, Entity or Value Object.
- A domain model is composed of domain objects.
- Composing the logic inside a domain model is often about finding a balance between **Purity** and **Completeness**.
  - In cases where we don't need to interact with out-of-process dependencies, we can have both.
  - In cases where we do need to interact with out-of-process dependencies, our decisions will lean towards **purity** or **completeness**.
- If we lean towards **completeness** we either:
  - Sacrifice **purity** by injecting out-of-process infrastructure into our domain model objects
    - Allows us to perform the operations we need against the out-of-process infrastructure in the domain object directly.
    - Business rules can then be applied against the results of those operations, directly inside the domain model
    - **Completeness has been maintained** by keeping rules inside the domain object.
    - But, **purity has been lost** by injecting out-of-process dependencies.
  - Keep **purity**, but **potentially sacrifice performance**, by injecting the data we need:
    - Operations on against the out-of-process infrastructure happens outside of the domain model.
    - The results of those operations are then injected into the domain model.
    - **Completeness has been gained** by keeping rules inside the domain object.
    - **Purity has been maintained** by the information required by those rules being injected into the domain model.
    - But, in some cases **performance may be lost** if there is a lot of information required to be injected into the domain model.
      - Getting all the users out of a database just and injecting them all into a domain entity, just so that the "email must be unique" business rule is kept inside the domain model is horribly detrimental to performance.
      - In this case, we have to sacrifice completeness by taking this business rule out of the domain layer.
      - In this particular case, a middle ground could be found where the query to see if an email is unique is done by a repository in the application layer, and the result of that query is passed into the domain model, if it makes sense to do so.
- Therefore, the closer our decisions lead towards completeness, the less performant the system may become.