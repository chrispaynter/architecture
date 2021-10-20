# CQRS

## Resources

- [CQRS](https://martinfowler.com/bliki/CQRS.html) by Martin Fowler

## Key Idea

- You can use a different model to update information than the model you use to read information.When to use it
  - Like any pattern, CQRS is us

## When to use it
- Like any pattern, CQRS is useful in some places, but not in others.
- Many systems do fit a CRUD mental model, and so should be done in that style.
- In particular CQRS should only be used on specific portions of a system (a BoundedContext in DDD lingo) and not the system as a whole. 
- 	In this way of thinking, each Bounded Context needs its own decisions on how it should be modeled.

### Benifits

- Some complex domains may be easier to tackle by using CQRS.
  - Martin Fowler suggests that the suitability of CQRS is a minor case.
- Handling high performance applications.
  - Separate the load of writes from reads.
  - Scale reads out if necessary.

## What to consider

- Some information systems 
