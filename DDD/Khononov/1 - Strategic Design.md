# Part 1 - Analysing Business Domains

[Book](https://learning.oreilly.com/library/view/learning-domain-driven-design/9781098100124/ch01.html#what_is_a_business_domainquestion_mark)

[TOC]

## What is a business domain?

- Company's main area of activity.

## What is a subdomain?

- To achieve it's business goals and targets, a company has to operate in multiple subdomains.
  - A fine grained area of business activity.
  - Composed together forming the company's **business domain**.
- Implementing a single subdomain is not enough for a business to succeed.
- The sub domains have to interact with each other to achieve the company's goals in the business domain.
- For example, Starbucks
  - Has to make great coffee
  - has to find retail locations and rent/buy them
  - has to hire people
  - etc
- None of those subdomains on their own make a great business.

### Types of subdomains

- Subdomains bear different strategic/business values.

#### Core Subdomains

- What a company does differently than it's competitors.
  - May involve inventing new products or services
  - May involve reducing costs by optimising existing services.
- **Core Domains** are **Core Subdomains**. The terminology is overlapping.
- A core subdomain can evolve into a generic subdomain over time.
- **Complexity**
  - **💡A core subdomain that is simple to implement can only provide a short-lived competitive advantage.**
  - Therefore, core subdomains are naturally complex.
  - There should be a high entry barrier for a company's core business.
    - It should be hard for competitors to copy or imitate the company's solution.
- **Sources of competitive Advantage**
  - Not all core subdomains are technical.
  - For example, a jeweler with an online store.
  - The online store is important, but the jewelery stuff is more crucial.

#### Generic Subdomains

- Business activities that all companies are performing in the same way.
- Like core subdomains, generic ones are generally complex and difficult to implement.
- However, they don't provide any competitive edge for the company.
- No need for innovation or optimisation here.
  - Battle tested implementations are available, and all companies use them.
- Examples:
  - Authentication and Authorisation 
  - Billing

#### Supporting Subdomains

- Support the company's business.
- Unlike the core domain, they don't provide any competitive advantage.
- Supporting subdomains are simple.
- Business logic is mostly data entry screens and ETL (extract, transform, load) operations
  - i.e CRUD interfaces
- These areas do not provide any competitive advantage for the company.

### Comparing Subdomains

#### Competitive advantage

- Only core subdomains provide competitive advantage
  - They're the company's strategy for differentiating itself from competitors.
- Generic subdomains by definition cannot be a source for competitive advantage
  - They're generic, off the shelf solutions.
- Supporting subdomains have low entry barriers and can't provide competitive advantage either.
  - In reality, we'd actually prefer to make them a generic subdomain if it's possible to find something off the shelf.
- The more complex the problems a company can tackle, the more business value they can provide.
  - Complex problems aren't limited to products and services.
  - They can be related to making the company more efficient.
    - i.e providing the same level of service as competitors, but at a lower cost.

#### Complexity

- **💡 When designing software, we have to choose tools and techniques that can accomodate the complexity of the business requirements.**
  - Therefore, identifying subdomains is essential for desiging a sound software solution.
- **💡Supporting subdomains business logic is simple.**
  - Possibly bespoke to the domain in question.
  - Not common enough that there is an off the shelf solution.
  - When it comes to tactical design, you may actually go as far as using transaction script instead, as the supporting domain is so simple.
    - i.e in a CRUD like fashioned, as opposing to investing in a richer domain model.
- **💡Generic subdomains are much more complicated.**
  - There should be a good reason why others have already invested time and effort into solving these problems.
  - These solutions are neither simple nor trivial
    - For example, consider encryption algorithims or authentication mechanisms.
- Core subdomains are complex
  - They should be as hard as possible for competitors to copy.
  - Profitability depends on this.
  - That's why strategically companies are looking to solve complex problems as their core subdomains.
- At times it can be challinging to differentiate between core and supporting subdomains
  - Complexity is a useful guiding principal.
  - Ask if the subdomain could be turned int a side business.
    - Would someone pay for it on it's won?
    - If they would, this is a core subdomain.
  - Similar reasoning between supporting and generic subdomains
    - Would it be simpler and cheaper to hack your own implementation, rather than implement an external one?
    - If so, this is a supporting subdomain.
- It's important to identifiy the core subdomains whose complexity will affect software design
  - A core subdomain is not necessarily related to software.
  - **💡 Another guiding principle to identify software-related core subdomains is toe value the complexity of the business logic that you will have to model in code.**
    - Does the business logic resemble a CRUD interface for data entry?
    - Or do you have to implement complex algorithms or business processes orchestrated by complex business rules and invariants?
    - In the formare case, it's a sign of a supporting subdomain.
    - The latter, a core subdomain.

#### Volatility

- Core subdomains can change often.
- **💡If a problem can be solved on the first attempt, it's probably not a good competitive advantage.**
  - Competitors will catch up fast.
- 💡**Consequently, solutions for core subdomains are emergent.**
  - Different implementations have to be tried out, refined and optimized.
- Work on core subdomains is never done.
  - Continously innovate and evolve core subdomains.
  - Add new features or optimise existing functionality.
  - Constant evolution of core subdomains is essential to stay ahead of competitors.
- Contrary to this, supporting subdomains do not change often.
  - They don't provide any competitive advantage for the company.
  - Evolving them provides miniscule business value compared to the same effort invested into a core subdomain.
- Generic subdomains can change over time
  - Security patches, bug fixes, or even totally new solutions

#### Solution Strategy

- All subdomains are required for the company to work in its business domain.
- 💡**The subdomains are like foundational building blocks.**
  - Take one away and the whole structure may fall down.
- Core subdomains have to be implemented in-house.
  - They can't be bought or adopted.
  - That undermines the notion of competitive advantage.
  - Competitors can just do the same otherwise.
- It's also unwise to outsource the implementation of a core subdomain.
  - It's a strategic investment.
  - Cutting corners on a core subdomain is not only risky in the short term
  - It can have fatal consequences in the long run.
    - Unmantiainable codebases that cannot support the company's goals and objectives.
  - The organisation's most skilled talent should be assigned to work on its core subdomains.
- **As core subdomains' requirements are expected to change often and contiously, the solution must be easy to maintain and evolve.**
  - 💡**Thus, core subdomains require implementation of the most advanced engineering techniques.**
- Generic subdomains are hard, but already solved problems
  - It's more cost effective to buy off the shelf, or go open source
- Lack of competitive advantages makes it reasonable to avoid implementing supporting subdomains in-house
  - However, unlike generic subdomains, no ready-made solutions are available.
  - No choice but to implement itself.
  - Though, the simplicity of the business logic and infrequency of change makes it easy to cut corners.
  - Don't require highly skilled technical aptitude
  - Good opportunity to train up-and-coming talent.
  - Also, the simplicity makes supporting subdomains a good candidate for outsourcing.

### Identifying Subdomain Boundaries

- Identifying subdomains and their types can help considerably in making different design decisions when building software solutions.
- Subdomains and their types are defined by the company's business strategy
  - it's business domains and how it differentiates itself to compete with outher companies in the same field.
  - In vast majority of software projects subdomains are "already there"
- A good starting point is the company's departments and other organisational units.
  - For example, online retail shop:
    - Warehous
    - Customer Service
    - Picking
    - Shipping
    - Quality Control
    - etc
  - These are relatively course-grained areas of activity.
  - Some are often outsourced to third party vendors, thus are supporting or even generic subdomains.
    - For example, a **Customer Service department**.

#### Distilling subdomains

- Course-grained subdomains are a good starting point.
  - But the devil is in the details.
  - We have to be sure we are not missing important information hidden in the intricacies of the business function
- A typical customer service department
  - Composed of finer-grained components (help desk system, shift management and scheduling, telephone system, and so on)
  - When viewed as individual subdomains, these activities acn be of different types
- Partitioning a coarse-grained subdomain into finer-grained subdomains helps you define further whether there are any other core, supporting or generic subdomains.

### Subdomains as coherent use case

- 💡**From a technical perspective, subdomains resemble sets of interrelated, coherent use cases.**
  - Such sets usually involved the same actor, the business entities, and they all manipulate a closely related set of data.
- We can use the definition of "subdomains as a set of coherent use cases" as a guiding principle for when to stop looking for finer-grained subdomains.
  - These are the most precise boundaries of the subdomains.
- This distillation is definitely necessary for core subdomains.
  - They're the most important, volatile and complex.
  - It's essential that we distill them as much as possible
    - It will allow us to extract all the generic and supporting functionalities
    - Then focus on the most valuable functionality.

### Who are the Domain Experts?

- They represent the business.
- They identified the problem in the first place.
- Whom all business knowledge originates from.
- As a rule of thumb, domain experts are either:
  - The people coming up with requirements
  - The software's end users.
- The software is supposed to solve their problems.
- Domain experts can have different scopes
  - Some might have a global view
  - Others might have a deeper view on a particular verticle.

