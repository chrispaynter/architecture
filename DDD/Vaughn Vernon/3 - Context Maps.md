# 3 - Context Maps

[TOC]

- This chapter focuses on the solution space assessment
  - Previous ones focused on problem space assesment

## Why Context Maps Are So Essential

- Your map is primary drawn to give your team the solution space perspective it needs to succeed.
- Other teams may not be using DDD, and may not care about your perspective.
  - For example, you may have to interface with a **Big Ball of Mud**.
  - The team maintaining it may not care what direction your project takes
  - As long as you adhere to their API
- However, your map gives your team the insight into where inter-team communication is imperative.
- You may hope for a **Customer-Supplier** relationship with the GBoM team.
  - However, turns out they only intend to provide what they have
  - Forces you into a **Conformist** relationship.

## Projects and Organisational Relationship

There are several DDD organisational and integration patterns. One of which commonly exists between any two Bounded Contexts.

- **Partnership**
  - When teams in two Contexts will succeed or fail together
  - A cooperative relationship needs to emerge.
- **Shared Kernal**
  - Sharing part of the model and associated code forms a very intimate interdependency
  - Can leverage design work or undermine it.
  - Designate with an explicit boundary some subset of the domain model that the teams agree to share.
  - Keep the kernal small.
  - Kernal has special status, cannot change without consulting all teams.
  - Align the teams around the Ubiquitous Language inside the kernal.
- **Customer-Supplier Development**
  - When two teams are in an upstream/downstream relationship.
  - Upstream team may succeed interdepenently of the downstream team.
  - Downstream team is dependent on the upstream team for developmental support.
  - Downstream priorities factor into upstream planning.
  - Budgeting and scheduling probably required.
- **Conformist**
  - When two teams are in an upstream/downstream relationship.
  - Upstream team has no motivation to provide for the downstream team's needs.
  - Downstream team is helpless.
  - Promises my be made from altruism, but unlikely to be fulfilled.
  - Downstream team eliminate complexity of translation by adhering slavishly to the upstream model.
- **Anticorruption Layer**
  - Translation layers can be simple and elegant when bridging well-designed Bounded Contexts with cooperative teams.
  - However, when control or communication is not adequate to pull of a shared kernal, partner or customer-supplier relationship, translation becomes more complex.
  - As a downtr
- Open Host Service
- Published Language
- Separate Ways
- Big Ball of Mud

