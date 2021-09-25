# Shared Kernal

## Sharing Ids between contexts

> It is recommended to not share models between bounded contexts, however, you can share IDs and even simple Value objects (like `Money`).
>
> The general rule is that **you may share anything that is immutable** or that changes very infrequently and IDs rarely change structure (here immutability refers to the source code and value immutability).
>
> https://stackoverflow.com/a/45274918/518341

- Ideally, the shared kernel will consist only of integration contracts and data structures that are intended to be passed across the bounded contexts’ boundaries.
  - https://www.oreilly.com/library/view/learning-domain-driven-design/9781098100124/ch04.html