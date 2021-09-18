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

- The emphasis of DDD is to invest our efforts in what matters most to the business.
- We focus predominantly on our core domain.
- Supporting models may not be given the same priority and effort.
  - They may even be outsourced to off the shelf solutions.
- Our focus will be on what distinguishes our business from all others.
- We'll be investing mostly into what is needed to achieve competitive advantage.

### 2. A refined, precise definition and understanding of the business is developed

- Can help business understand itself and its mission better than before.
- As model is refined over time, business develops a deep understanding.
- Details surfacing from domain experts can be challenged and refined by others.

### 3. Domain experts contribute to software design

- Domain experts can have different views, opinions, backgrounds.
  - Different experiences before joining the organisation for example
  - Divergent paths within the same organisation
- When brought together via a DDD effort, domain experts gain consensus among themselves.
- Fortfifies the effort and the organisation as a whole.
- Developers now share a language with the domain experts.
  - Benefit further from knowledge transfer from the domain experts.
  - As developers move on training and handoffs are easier
- Less chance of "tribal knowledge", where only a select few understand the model, are reduced.
- The knowledge is available from anyone else in the organisation.

### 4. A better user experience is gained

- More expressive models lead to more expressive user experiences.
- Moving away from CRUD, where users have to string together workflows themselves.

### 5. Clean boundaries are placed around pure models

### 6. Enterprise architecture is better organised

- Enterprise architecture is easier to follow
- Boundaries are explicit, as are the relationships between them

### 7. Agile, Iterative, Continous Modeling is Used

- DDD is about refining the mental model of domain experts into a useful model for the business.
- It's not about creating a real world model, as in trying to mimic reality.
- The model is produced iteratively and incrementally.
- It's refined continously until it's no longer needed by the business.

### 8. New tools, both strategic and tactical, are employed

## The challenges of applying DDD

- Some common ones
  - Allowing for the time and effort required to create a Ubiquitous Language
    - To apply DD completely, with the greatest value to the business, it's going to require more thought and effort.
    - That takes time. That's the way it is, period.
  - Involving domain experts at the outset and continuously with the project
    - No matter the challenges procuring the experts, this is crucial.
    - At least one, or you won't get deep knowledge of the domain.
    - Developers must converse and listen with the true experts and mold their spoken language into software that reflects their mental model of the domain.
  - Changing the way developers think about solutions in their domain

