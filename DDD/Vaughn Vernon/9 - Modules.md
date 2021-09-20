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
  - Peer modules are those that are on the same "level"
    - ⚠️ **He mentions the idea of parent child modules. I did not know about these in DDD. Needs more thought.**
  - It's rarely possible or even practical for Modules to be completely independent of each other.
  - After all, a domain model implies some coupling.
  - Yet, reduce the coupling of component if you make the dependency between two peer Modules unidirectional.
  - For example, product depends on team, but team does not depend on product.

