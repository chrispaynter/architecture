# Guide to Implementing DDD

## Big-Picture view of DDD

- UL is applicable within a single bounded context
- Tactical - the actual coding of the model, the way you choose to
- Strategic - following the concepts of DDD

## Strategic Modelling

- **Bounded Context** (BC) is a conceptual boundary where a domain model is applicable.
- The BC provides context for the **Ubiquitous Language** (UL) spoken by the team.
- The UL is expressed in its carefully designed software model.
- **Context Mapping** (CM) are used to show the relationships between BCs.
  - Integration relationships between BCs include Open Host Service, Published Language, Anticorruption Layer, Customer-Supplier Partnership, Conformist, Shared Kernal.
- **Strategic design helps us to determine:** 
  - what the most important software investments are to make.
  - What existing software assets to leverage in order to get there faster and safest
  - Who must be involved

## Architecture

- Domain models should be architecturally neutral.
- A BC is hosted inside the arhitecture.
- Architecture is important, but more emphasis should be place on domain model
  - Greater business value
  - More enduring

## Tactical Modelling

- We model tactically inside of a BC using DDD's building block patterns
- **Aggregate** is one of the most important design patterns
  - Composed of single **Entity**
  - Or, a cluster of entities and **Value Objects**
    - must remain transactionally consistent throughout the aggregate's lifetime.
- An instance of an Aggregate is persisted using it's **Repository**.
- **Services** are used to perform business operations that don't fit naturally as an operation on an Entity or a Value Object.
- Use **Domain Events** to indicate the occurence of significant happenings in the domain.
- Use **Modules** to package a limited set of cohesive domain objects.
- **Tactical design helps us craft the single elegant model of a solution using time-tested, proven software building blocks.**



