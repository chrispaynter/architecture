# 10 - Aggregates

[TOC]

## Rule: Model True Invariants in Consistency Boundaries

### What is an invariant?

- An invariant is a business rule that must always be consistent.
- **There are different types of consistency:**
  - **Transactional Consistency**
    - Immediate and Atomic
  - **Eventual Consistency**
    - Not imediate
- When we discuss invariants, we are talking about transaction consistency.

### Constency Boundary

- Logically asserts that everything inside adheres to a specific set of business invariant rules, no matter what operations are performed.
- The consistency of everything outside this boundary is irrelevant to the Aggregate.
- 💡 Thus, *Aggregate* is synonymous with ***transactional consistency boundary***.

### Transactions

- We use a single transaction to managed consistency (such as a Unit of Work)
- When the transaction commits, everything inside one boundary must be consistent.

> A properly designed Aggregate is one that can be modified in any way required by the business with its invariants completely consistent within a single transaction.

- 💡 A properly designed Bounded Context modifies only one Aggregate instance per transaction, **in all cases.**

> We cannot correctly reason on Aggregate design withou applying transactional analysis.

- Limiting modification to one Aggregate instance per transaction may sound overly strict.
  - It is a rule of thum and should be the goal in most cases.
  - It addresses the very reason to use Aggregates.

#### User Interface should concentrate each request based on Aggregates

- The fact that Aggregates must be designed with a consistency focus implies that the UI should concentrate each request to execute a single command on just one Aggregate instance.
- If user requests try to do too much, the application will be forced to modify multiple instances at once.
- **💡 Therefore, Aggregates are chiefly about consistency boundaries and not driven by a desire to design object graphs.**
  - Though, some real world invariants will be more complex than this.
  - Even so, typically invariants will be less demanding on our modeling efforts, making it possible to design *small Aggregates*.

## Rule: Design Small Aggregates

- Even if you could guarantee a large cluster would succeed, a large cluster still limits performance and scalability.
- The older an instance of a large aggregate gets, the more data that is being retrieved from the DB potentially
  - Consider collections of sub items that has more than a thousand entries.
  - We may never have to load all these at one time based on business rules.
  - However, the aggregate design forces as to everytime we want to add a new sub item.
- The choice to include a collection of items onto an entity has to support some invariant in the Aggregate.
  - Including it just for compositional choice is a no-no.
- **What does small mean?**
  - **💡Limit the Aggregate to just the Root Entity and a minimal number of attributes and/or Value-typed properties.**
    - Value-typed properties being an attribute that holds reference to a Value Object.
  - Which ones are necessary?
    - Those which must be consistent with others.
    - This isn't always explicitly defined by domain experts.
    - For example, a `name` and `description` property on a `Product` aggregate
      - You couldn't imagine those being separated into seperate aggregates.
  - **What if you think you should model a contained part as an entity?**
    - **💡First ask wether that part must change over time, or whether it can be completely replaced when change is necessary.**
      - Cases where complete replacement is adequate point to use of a Value Object.
      - At times, Entities are necessary.
      - Yet, running through this design exercise on a case by case basis can lead to many objects modelled as Entities to be modelled as Value Objects.
  - **Benefits of favouring Value Objects**
    - Value Objects don't required separate tables (necessarily)
      - Less JOINs, faster queries
    - Value Objects are smaller and safer to use
      - Easier to test, due to immutability
  - 💡 **The book indicates that a high percentage of Aggregates can be limited to a single Entity, the Root.**
- Smaller Aggregates not only perform and scale better, they are also biased towards transactional success, meaning that conflicts preventing a commit are rare.

### Don't trust every use case

- Business analysts play an important role in delivering use case specifications
  - They will affect many of our design decisions.
  - Remember, use cases designed this way don't carry the perspective of the domain experts and developers of our close-knit modeling team.
  - We still must reconcile each use case with our current model and design, including our decisions about Aggregates.
  - ⚠️ **A common issue is that a particular use cas calls for the modification of multiple Aggregate instances.**
    - We must determine whether the specified large user goal is spread across miltple persistence transactions, or if it occurs within just one.
      - If it's the latter, it pays to be skeptical.
      - No matter how well it's written, it may not accurately reflect the true Aggregates of our model.
- A new use case _may_ lead to insights that push us to remodel the Aggregates in question.
  - For example, the use case needing multiple aggregates in one transaction
  - Could indicate some, or all, of one Aggregate gets rolled into another Aggregate to keep a consistency boundary.
  - Though be skeptical.
  - Could also lead to creating a large cluster Aggregate.
- 💡**Just because you are given a use case that calls for maintaining consistency in a single transaction, doesn't mean you should do that.**
  - Often, the business case can be achieved with eventual consistency between Aggregates.
  - You may have to rewrite the use case, specifying the use of eventual consistency and the acceptable update delay.

## Rule: Reference Other Aggregates by Identity

- One Aggregate may hold reference to the Root of other Aggregates.
  - Howevever, this doesn not place the referenced Aggregate *inside* the consistency boundary of the one referencing it.
  - The reference doesn not cause the formation of just one whole Aggregate.
- Holding a direct object association to an Aggregate that doesn not belong in the boundary of another has a few implications:
  - Both the referencing and the referenced Aggregate *must not* be modified in the same transaction. **Only one or the other may be modified in a single transaction.**
  - If you are modifying multiple instances in a single transaction, it may be a strong indication that your consistency boundaries are wrong.
- If you don't hold any reference, you can't modify another aggregate.
  - So avoiding the object reference to start with can help avoid the temptation to include multiple aggregates in a single atomic transaction.

### Making Aggregates Work Together through Identity References

- 💡Prefer references to external Aggregates only by their globally unique identify, and not by holding a direct object reference ("pointer")
  - The Aggregates with inferred object references are thus automatically smaller
  - References are never eager loaded
  - Less time to load, less memory.

### Model Navigation

- We obviously at times will still need to reference directly the related Aggregates.
  - Some use a **Repository** directly inside an Aggregate for lookup. 
    - This is not recommended, as reduces the "purity" of the model.
  - 💡The recommended approach is to use a Repository or **Domain Service** to lookup the dependent objects ahead of invoking the Aggregate behaviour.
  - A client Application Service may control this, then dispatch to the aggregate.
  - 💡**For some very complex and domain-specific dependency resolutions, passing a Domain Service into an Aggregate command method can be the best way to go.** [ONE BASKET]
    - The aggregate can then *double-dispatch* to the Domain Service to resolve references.
  - Again, no matter how you provider the referenced Aggregate, it doesn not mean both should be modified inside a single transaction. In fact, the referenced Aggregate should not be modified at all.