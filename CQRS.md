# CQRS

## Resources

- [CQRS](https://martinfowler.com/bliki/CQRS.html) by Martin Fowler

## Key Idea

- You can use a different model to update information than the model you use to read information.When to use it

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

- Some information systems fit well with the notion that information is updated in the same way that it is read.
- CQRS is just a tool in the toolbox.
- An entire system doesn't have to be built using the pattern.
- Maybe just some parts of the system make more sense.

## Synchronicity vs Asyrchronicity

- Commands don't have to be processed immedaitely - they can be queued.
- How fast they get processed is a matter of SLA and is not architecturally significant.
- In this way, commands are autonomous - they're sent off and trusted.
- UIs may not even show anything else to the user once the command is sent off (i.e no list of pending commands)

## CRUD vs CQRS

- Take for example, a customer update process.
- In CRUD, you might have a single form with all fields on the customer. One of those fields might be "make preferred".
  - All this data gets saved at the same time, in a single unit of work.
- Consider that "make preferred" gets selected, and saved.
  - In between opening the customer record and saving, an event arrived saying the user doesn't pay their bills.
  - In that case, the customer should not have been made preferred.
  - However, in the CRUD system, we're just updating fields in a database.
  - Unless we added transaction script type logic to the update process, the user may be made preferred when they shouldn't.
- In CQRS, we split these concepts into commands
  - Task based operations, each a unit of work in their own right.
  - The update of basic data like name, email, etc could be one command.
  - The changing of preferred status would be another.
  - Each having their own individual business logic.

## Other things to note

- Commands are _sent_ to the server.
- Not _published_.
  - Publishing is used for events, a separate idea.
