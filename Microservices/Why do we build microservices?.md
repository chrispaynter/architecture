# Why do we build microservices

## First, the bounded context

- General idea is each business domain is thought of as its own self-contained system 
  - with judiciously-designed inputs and outputs.
- From an organizational standpoint, each bounded context is owned by a single, cross-functional team. 
- The team builds, deploys, and maintains the services and applications that it needs to get its job done
  - with minimal dependencies on any other teams.

## Most fundamental benefit of bounded contexts

- There are many technical benefits that could be rattled off:

  - Separation of concerns
  - Continous Deployment
  - Decoupling

- Howevere, ["The true benefits are organisational"](https://betterprogramming.pub/the-truth-about-your-source-of-truth-a1eb833c2d70)

  - Teams have the control and responsibility over their own projects.

  - They can make as many changes to their code as they want to, with little fear of breaking other teams.

  - They can release as frequently as they need, without requiring coordination with any other team. 

  - they own the entire lifecycle of their code, from design and development, to deployment, to production monitoring.

  - > Enforcing a **single source of truth** for our domain entities sabotages those benefits.