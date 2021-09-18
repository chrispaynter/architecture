# Getting started with DDD

- We primarily want to use DDD in the areas that are most important to the business.
- Don't invest in what can be easily replaced.
- *Invest in the nontrivial, the more complex stuff, the most valuable and important stuff that promises to return the greatest dividends.*
- We call such a model the **Core Domain**.
- In second priority are the *significant* **Supporting Subdomains**.
- These get the most investment.

#### Use DDD to simplify, not to complicate

- Use DDD to model a complex domain in the simplest possible way.
- Never use DDD to make your solution more complex.

## Ubiquitous but not Universal

- Ubiquitous is not a term to describe an enterprise-wide, company-wide or worldwide, universal domain language.
- Ther is one Ubiquitous Language per **Bounded Context**.
- Bounded Contexts are relatively small. Large enough only to capture the complete UL of the isolated business domain.
- The language is ubiquitous only within the team that is working on the project, that developes an isolated bounded context.
- On a project that builds a single BC, there are always one or more additional isolated BCs with which it integrates using **Context Maps.**
  - Each one has it's own UL, even though some terms may overlap.
- If you try to apply a single UL to an enterprise or worse multiple enterprises, **you will fail**.

## The business value of using DDD

### 1. The organisation gains a useful model of it's domain

### 2. A refined, precise definition and understanding of the business is developed

### 3. Domain experts contribute to software design

### 4. A better user experience is gained

### 5. Clean boundaries are placed around pure models

### 6. Enterprise architecture is better organised

### 7. Agile, Iterative, Continous Modeling is Used

### 8. New tools, both strategic and tactical, are employed

