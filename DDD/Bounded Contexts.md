# Bounded Contexts

[TOC]

## Resources

- [If You’re Building Microservices, You Need to Understand What a Bounded Context is](https://medium.datadriveninvestor.com/if-youre-building-microservices-you-need-to-understand-what-a-bounded-context-is-30cbe51d5085)
  - This contains a nice top level overview of peeling away microservices from a monolith.
  - ![img](https://miro.medium.com/max/1400/1*r2Y9wky92C4zgzzAC9_IvQ.png)

## Benefits of Bounded Contexts

- There are many technical benefits:

  - Separation of concerns
  - Continous Deployment
  - Decoupling

- ["The true benefits are organisational"](https://betterprogramming.pub/the-truth-about-your-source-of-truth-a1eb833c2d70)

  - Teams have the control and responsibility over their own projects.

  - They can make as many changes to their code as they want to, with little fear of breaking other teams.

  - They can release as frequently as they need, without requiring coordination with any other team. 

  - they own the entire lifecycle of their code, from design and development, to deployment, to production monitoring.

  - > Enforcing a single source of truth for our domain entities sabotages those benefits.

## Single Source of Truth

- We often try to keep data (i.e our Domain entities) owned and available from a single source of truth.
  - Common wisdom has it that every domain entity in our enterprise should live in exactly one centralized location. 
  - If you want to fetch an instance of that entity, you go to that location.

> As with much common wisdom, this isn’t necessarily so. It’s a good idea to keep track of where our data resides, sure. But bending over backward to define a single SoT for every piece of data is the wrong approach. At best, it’s often simply not necessary. Worse, it can cause more problems than it purports to solve. In fact, the very notion runs counter to the event-based systems that power many enterprises today. And as we’ll discuss, a true source of truth for our data is, by and large, mythical.