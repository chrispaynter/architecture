# 2 - Domains, Subdomains and Bouded Contexts

[TOC]

There are three things you have to understand very clearly:

- What your **Domain** is
- What your **Subdomains** are
- What your **Bounded Contexts** are.

## Big Picture

- A Domain, in the broad sense, is what an organisation does and the world it does it in.
- Each business has it's own unique realm of know-how and way of doing things.
- The term *domain model* whilst having the word domain in it, doesn't mean we create on huge model for the organisation's entire domain.
- The whole domain is composed of **subdomains**.
- Using DDD, models are developed in Bounded Contexts.
- Almost every software Domain has multiple Subdomains.
  - Doesn't matter the size of the organisation.
  - There are different functions that make business' successful
  - It's advantageous to think about each of those business functions separately.
- Any attempt to model even a moderately sized organisation in a single model will likely fail.

### Subdomains and Bounded Contexts at Work

- Unfortunately, most systems today are not created by employing DDD approach
  - Typical situation is we have fewer subsystems responsible for many business functions.

## Real-World Domains and Subdomains

- Domains have both a ***problem space*** and a ***solution space***.
  - Problem space allows us to think of a strategic business challenge to be solved.
    - The parts of the **Domain** that need to be developed to deliver a new **Core Domain**.
    - Involves examining Subdomains *that already exist, and those that are needed*.
    - Thus, problem space is the combination of the **Core Domain** and the **Subdomains** it must use.
    - **Subdomains** allow us to ropidly view different parts of the **Domain** that are necessary to solve a specific problem.
  - **Solution space** focuses on howe we will implement the software to solve the problem of the business challenge.
    - One or more **Bounded Contexts**, a set of specific software models.
    - The **Bounded Context** *is a specific solution*, a realisation view, once developed.
    - The **Bounded Context** is used to realize a solution as software.
- It's a desirable goal to align **Subdomains** one-to-one with **Bounded Contexts**.
  - Expressly segregates domain models into well-defined areas of business by objective
  - Melds the problem space with the solution space.
  - Sometimes this is not possible in legacy systems, where **Subdomains** my interesect **Bounded Contexts**.

### Assening the problem space and solution space

- Before we can execute a specific solution, we need to make an assessment of the problem space and the solution space.
- These questions should be answered in order to steer your project in the right direction.
  - What is the name of and vision for the strategic **Core Domain**?
  - What concepts should be considered part of the strategic **Core Domain**?
  - What are the necessary **Supporting Subdomains** and the **Generic Subdomains**?
  - Who should do the work in each area of the domain?
  - Can the right teams be assembled?
- If we don't understand the vision and goals of the **Core Domain** and the areas of the Domain that are needed to support it, we won't be able to strategically take advantage of them and avoid associated pitfalls.
- 