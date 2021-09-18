# 1. Getting started with DDD

[TOC]

- We primarily want to use DDD in the areas that are most important to the business.
- Don't invest in what can be easily replaced.
- *Invest in the nontrivial, the more complex stuff, the most valuable and important stuff that promises to return the greatest dividends.*
- We call such a model the **Core Domain**.
- In second priority are the *significant* **Supporting Subdomains**.
- These get the most investment.

**Use DDD to simplify, not to complicate**

- Use DDD to model a complex domain in the simplest possible way.
- Never use DDD to make your solution more complex.

## Can I DDD?

You can implement DDD if you have:

- A passion for creating excellent software every day, and the tenacity to achieve that goal.
- The eagerness to learn and improve, and the fortitude to admit you need to.
- The aptitude to understand software patterns and how to properly apply them.
- The skill and patience to explore design alternatives using proven agile methods.
- The courage to challenge the status quo
- The desire and ability to pay attention to details, to experiment and discover.
- A drive to seek ways to code smarter and better.

### DDD isn't first and foremost about technology:

- If you are capable of understanding the business in which your company works, you can at a minimum participate in the software model discover process to produc a **Ubiquitous Language**.

### But we don't have domain experts

- A domain expert is not one by job title.
- These are people that know the line of business you are in really well.
- They probably have a lot of background in he business domain, and they might be product designers or even sales people.
- Look past the job title.
- The people you are looking for know more about what you are working on than anyone else, and for sure more than you know.
- Find them. Listen. Learn. Design in code.

### What's a domain model?

- A software model of the very specific business domain you are working in.
- Often implemented as an object model, which has both data and behaviour.
- Carefully crafting a unique domain model at the heart of a core, strategic application or subsystem is essential to DDD.
- With DDD, domain models tend to be smallish and very focused.
- With DDD, you never try to model the whole business enterprise with a single, large domain model.

## Why you should do DDD

- **Put domain experts and developers on a level playing field.**
  - Produces software that makes sense to the business, not just the coders.
  - Cohesion.
- **It's an investment into the business.**
  - Software that's as close as possible to what the business leaders and experts would create if they were the coders.
- **Teaches the business more about itself.**
  - No single person is capable to know everything about the business.
  - Constant discovery process, becomes more insightful over time.
  - Everybody learns, because everybody contributes to discovery discussions.
- **Centralizes knowledge**
  - 

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
    - Some good information from pages 31 - 34
    - Touches primarily on the need to model behaviours in a rich way, and some techniques we use to do that.
    - Compares to the more tranactional script approach.

### Justification for domain modeling

- *Tactical* modeling is generally more complex than *strategic* modeling.
- Using the DDD tactical patterns (Aggregates, Services, Value Objects, Events, etc) will require:
  - more careful thought
  - greater investment

#### Criteria for justifying investment into tactical modeling

- The **core domain** is an unknown and complex area.
- The team is best protected against a disastrous failure if using the right tactics.

##### Some practical guidance

- If a Bounded Context is being developed as the Core Domain, that BC is strategically vital to the success of the business.
  - The core model is not well understood, will require lots of experimentation and refactoring.
  - Deservers commitment to longevity and continous enchancement.
  - If the BC is complex, innovative and needs to endure for a long time as it undergoes change, strongly consider the use of the tactical patterns as an investment into the future of your business.
  - The core domain deserves the best developer resources, with a high skill level.
- If you are developing a BC as your chief business initiative, it is your core domain regardless of how it is viewed by customers outside the business
  - Despite the fact that it may actually be viewed as a **Generic Subdomain** or a **Supporting Subdomain** to them.
  - Therefore, strongly consider the use of tactical patterns.
- If developing a Supporting Subdomain that cannot be acquired as a third-party Generic Subdomain, tactical patterns can benefit your efforts.
  - Consider if the model is new, and if it is innovative.
  - Consider the skill level of the team.
  - If it's innovative, it adds specific business value, captures special knowledge, and is not just technically intriguing.
  - If the team is capable of properly applying tactical design and the Supporting Subdomain is innovative and must endure for years in the feture, this is a good opportunity to invest in your software using tactical design.
  - However, this *does not* make this model the Core Domain in the eyes of the business, it is merely supporting.

##### Further decision parameters for using tactical design

- Are domain experts available and are you committed to forming a team aroud them?
- Even if the business domain is simple now, will it grow in complexity over time?
  - There is risk in using Transaction Script for complex applications.
  - If you use Transaction Script now, will the potential for refactoring to a behavioral domain model later on be practical?
- Will the use of DDD tactical patterns make it easier and more practical to integrate with other BCs, whether 3rd pary or custom developed?
- Will development really be simpler and require less code if you use Transaction Script?
  - Be cautios because the complexity of the domain model and the innovation of the model are often not well understood during project planning.
  - Underestimating domain complexity and the innovation involved often happen.
- Do the critical path and timeline allow for any overhead required for tactical investment?
- Will the tactical investment in a Core Domain protect the system from changing architectural influences?
  - Transaction Script may leave it exposed.
  - **Domain models are often enduring while architectural influnces tend to be more disruptive to other layers.**
- Will customers/clients benifit from a cleaner, enduring design and development approach?
  - Or could their application be replaced by an off-the-shelf solution tomorrow.
  - Why would we develop this as a custom application in the first place?
- Will using tactical DDD be more difficult than approaches such as Transaction Script?
  - **Skill level and availability of domain experts is vital to answering this question.**
- In the end, it's the business customer, and not the object practioners and technologists, who must be pleased.
- Choose wisely.

### DDD is not heavy

- It is meant to fit well into any agile project framework
- Its design tenets lean toward rather rapid test-first refinements of a real software model.

## Fiction, with buckets of reality

