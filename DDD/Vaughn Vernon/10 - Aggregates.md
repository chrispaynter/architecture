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
    - Consistency is eventually reached via asynchronous processing
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
- Holding just an identifier reference can make it harder to build views for UI purposes.
  - This is where CQRS can assist.

### Scalability and Distribution

- Since Aggregates don't use direct references to other Aggregates, but reference by identify, their persistent state can be moved around to reach large scale.
- 💡Since there are always multiple Bounded Contexts in play in a given Core Domain, reference by identity allows distributed domain models to have associations from afar.
  - **When an Event-Driven approach is used, message-based Domain Events containing Aggregate identities are sent around the enterprise.**
  - Message subscribers in foreign Bounded Contexts use the identities to carry out operations in their own domain models.
  - Reference by identity forms remote associtaions or *partners*
  - 💡Transactions across distributed systems are not atomic. The various systems bring multiple Aggregates into a consistent state eventually.

## Rule: Use Eventual Consistency Outside the Boundary

- > Any rule that spans AGGREGATES will not be expected to be up-to-date at all times. Through event processing, batch processing, or other update mechanisms, other dependencies can be resolved within some specific time. [Evans, p. 128]

- This, if executing a command on on Aggregate instance requires that additional business rules execute on one or more other Aggregates, use eventual consistency.

  - Accepting that all Aggregate instances in a large scale high traffic enterprise are never completely consistent helps us accept that eventual consistency also makes sense in the smaller scale where just a few instances are involved.

- Ask domain experts if they can tolerate some time delay between the modification of one instance and the others involved

  - Ofter they're willing for reasonable delays - generous number of seconds, minutes, hours, even days - before consistency occurs.

- **Domain Events are a way to support eventual consistency in DDD.**

  - The original aggregate being changed fires an event.
  - Event is in time delivered to one or more asynchronous subscribers.
  - Each subscriber then retrieves a different yet corresponding instance of the original aggregate and performs it's behaviour based on it.
  - Each subscriber performs in a separate transaction, obeying the rule to modify one instance only per transaction.
  - 💡If a subsriber fails for "reasons", then a retry process can attempt to fire it again.
  - 💡If complete failure happens, then it may be necessary to compensate, or at the least report the failure for pending intervention.
  - 💡These techniques can be used within a single Bounded Context, but also can be used in a distributed fashion.
    - Note - NServiceBus could be used in both situations.

## Reasons to break the rules

- When two allow multiple aggregate transactions
- **Reason One: User Interface Convenience**
	- Sometimes UI interfaces batch creation of aggregates
	- If creating a batch of Aggregate instances all at once is semantically no different from creating one at a time repeatedly, it represents one reason to break the rule of thumb with impunity.
- **Reason Two: Lack of Technical Mechanisms**

  - Eventual consistency requires the use of some kind of out-of-band processing capability
    - Messaging, Timers or background hrteads
  - If the project you are working on has no provision for this, you might end up tempted to create a large-cluster Aggregate to compensate.
  - In this case, it is probably worth breaking the rule of thumb of one Aggregate per transaction.
- **Reason Three: Global Transactions**
- **Reason Four: Query Performance**
  - May be times when it's best to hold direct object references to other Aggregates.
  - Perhaps to ease Repository query performance issues.
  - Must be weighed carefully.

## Gaining Insight through Discovery

- There's a good fictional case study worth reading again and again in the book.
  - It runs through how the team determines the realities of the concerns below.

### Rethinking the Design, Again

- Focusing on small aggregates leads to the potential to overdo it.
  - Questioning each invariant - "is it a true invariant"?
  - Everytime we split the aggregate, we potentially get closer to needing to use eventual consistency
    - As per the rule, one transaction per Aggregate
- It's a balancing act of performance and scalability.

#### Estimating Aggregate Cost

- Large cluster aggregates have potential performance risks
  - Lots of data required, more memory used
  - More SQL querying required (joins) to build the bigger aggregate.

#### Common Usage Scenarios

- It's also worth asking:
  - "How many items could this sub collection end up having?"
  - "How often does all this data need to be loaded"?
  - "Would there typically be multiple client usage that causes concurrency contention on the data"
- If you're loading a largish sub collection of items each time the Aggregate is loaded, but they're never needed, it could be a sign that they 

#### Implementing Eventual Consistency

- If it's deemed worth separating out an Entity as it's own Aggregate, and there is an existing invariant that needs to be maintained, then the need to deal with eventual consistency has arrived.
  - The book has an example of a BackLogItem in a Jira like system
  - BackLogItem aggregate contains Tasks
  - The Tasks get factored out
  - When all tasks are done, the BackLogItem status needs to be set to "Done"
  - Task fire an event that triggers a listener
  - The listener calls into a domain service that figures out whether the BackLogItem should be set to done
  - or something like that

### Time for decisions

- Analysis can't continue all day.
- There needs to be a decision.
- Aim for choices that don't negate the possibility of going another route later.

## Implementation

- **Create a root with Unique Idenity**
- **Favour Value Object Parts**
  - Choose Value Objects over Entities whenever possible
- **Using Law of Demeter and Tell, Don't Ask**
  - Both these design principles stress information hiding.
  - LOD
    - Emphasises the *principle of least knowledge.*
  - Tell, Don't Ask

### Optimistic Concurrency

- Read the book
  - Interesting [ideas here](http://www.kamilgrzybek.com/design/handling-concurrency-aggregate-pattern-and-ef-core/).

### Avoid Dependency Injection

- Dependency injection of a Repository or Domain Service into an Aggregate should generally be viewed as harmful.
- The motivation may be to look up a dependent object instance from inside the Aggregate.
  - Another Aggregate, or a number of them.
  - https://stackoverflow.com/questions/5694241/ddd-the-rule-that-entities-cant-access-repositories-directly
- 