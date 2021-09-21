# 9 - Modules

Original DDD book points on modules

## Simple Rules for Module Design

- **DO design Modules to fit modeling concepts**
  - Typically you'll have one Module for one or a few **Aggregates** that are cohesive, if only by reference.
- **DO name modules per the Ubiquitous Language**
  - This is a basic goal of DDD, but will also tend to come naturally if you think about the concepts being modeled.
- **DON'T create Modules mechanically according to a general component type or pattern being used in the model.**
  - Don't segregate into "Aggregates", "Services", etc modules.
  - This misses the point of DDD modules.
- **DO design loosely coupled modules**
  - Make it easier to maintain and refactor your modeling concepts
- **DO strive for acyclic dependencies on peer Modules where coupling is necessary.**
  - Peer modules are those that are on the same "level" conceptually
  - It's rarely possible or even practical for Modules to be completely independent of each other.
  - After all, a domain model implies some coupling.
  - Yet, reduce the coupling of component if you make the dependency between two peer Modules unidirectional.
  - For example, product depends on team, but team does not depend on product.
- **DO relax the rules a bit between child and parent Modules.**
  - By child and parent, it's not to say the modules still aren't on the same "level" from a file system perspective, but more so that conceptually one is naturally the child of another.
  - It's difficult to prevent dependency between parent and child Modules.
  - Strive for acyclic, but allow for circular if there's no way to avoid it.
    - A parent creates a child
    - The child must reference it's parent (if only by identity)
- **Don't make Modules a static concept of the model, allow them to be molded along with the objects that they organise.**
  - If the model's concepts are malleable and take on different shapes, behaviours and names over time, it's likely that the Modules that organise those same concepts should be created, renamed and deleted in kind.
  - Refactoring is a pain of course, but probably less than that experienced with poorly name Modules.

## Modules are first class citizens of the model

- Kitchen drawer analogy, where one drawer has sharp knives, tea spoons, hammers and sockets.
- A more orgainesed approach to the drawers makes the kitchen nicer to use.
- On the other hand, drawers that are organised based on heavy items, breakables, probably don't work well either
- Consider
  - "Placesettings" module
    - Fork, Spoon, Knife
    - Perhaps even napkins
  - Makes more sense than modules named "blunt", "scooping", "pronged", etc

## Basic module naming conventions

- Read the book

## Module before Bounded Context

- When it is not clear if contextual boundaries should be created, first consider the possibility of keeping them together.
  - **💡 This approach will use the thinner boundary of Module to separate, rather than the thicker one of Bounded Context.**
- Bounded Context's are not a substitute for Modules
- Use Modules to modularize cohesive domain objects, and to separate those that are not cohesive or less cohesive.
- Thus, Modules can be used to separate subdomains from the Core, without having to house them in their own external sub system.



