# 3 - Managing Domain Complexity

## Boundend Contexts vs Subdomains

- It's crucial to remember that subdomains are discovered and bounded contexts are designed.
  - The subdomains are defined by the business strategy.
  - However, we can design the software and it's bounded contexts to address the specific project's context and constraints.

## Boundaries

### Physical Boundaries

- Bounded contexts serve not only as model boundaries, but also as physical boundaries of the systems implementing them.

  - Each bounded context should be implemented as an individual service/project
  - It's implemented, evolved and versioned independently of other bounded contexts.

- 💡**A bounded context can contain multiple subdomains.**

  - In such a case, the BC is a physical boundary
  - Each of it's subdomains is a logical boundary.

- A bounded context should be owned and developed by one team only.

  - However, a team can own more than one BC

  