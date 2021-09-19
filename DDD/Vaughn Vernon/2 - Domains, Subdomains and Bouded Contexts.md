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
  - 