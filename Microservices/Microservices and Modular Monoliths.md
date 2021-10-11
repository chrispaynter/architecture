# Microservices and Modular Monoliths

[TOC]

# Introduction

This is an attempt to aggregate information about both architectural styles in a way that can be referenced as a guide during architectural thinking.

- It's goal is to bring microservices thinking into the design of a monolithic application.
- The intention being to design and build a monolith that can have individual modules broken off into microservices, when the time comes.

## Cost comparison over time

<img src="/Users/chrispaynter/Downloads/Artboard.jpg" alt="Artboard" style="zoom: 67%; float:left;" />

## What are we trying to achieve with a Modular Monolith?

- Craft module boundaries in a way that provide all the benefits of microservices.
  - Information hiding
    - Clients cannot access internals
  - Client code can only access a publicly exposed API
    - Inter-process for Microservices
    - In-process for modules
- Hiding internals helps to avoid creating a big ball of mud 
  - Difficult to break into microservices later if the time comes.

## Similarities

- Craft module boundaries in a way that provide all the benefits of 

## Differences

- Microservices expose a inter-process API, whereas monoliths expose an in-process API
  - In CQRS, the in-process API would generally be the commands and queries themselves.
  - Could add layer that mimics the REST APIs functionality in order to better represent the module as a "microservice"
    - Would ease transition later
  - Doing so would add more overhead to the monolith though, which may:
    - add confusion to new devs
    - erode some of the productivity savings.

# Primary Challenges

The primary challenges of proceeding with a microservices or modular monolith architecture are:

- Defining the service boundaries correctly
- Communicating between service boundaries
- Sharing code between boundaries
- Performing transactions across boundaries
- Versioning services/modules
- Architecting the overall system solution

# Boundaries

- There are technological _and_ logical benifits to crafting well designed boundaries.
- The microservices architecutre uses services as a unit of modularity.
  - "Componentisation via services"
- A service has an API, which is an impermeable boundary that is diffucilt to violate.
  - You can't bypass an API and access internal classes as you can with a Java package.
  - Thus, it's much easier to preserve the modularity of the application over time.
- What belongs inside a given microservice?
- What doesn't?

## Boundaries in a modular monolith

> Design as microservices, build as modules.

- We can apply the same approach to designing modules as we do with microservices.
- After all, in DDD speak, they're esentially bounded contexts.
- However, applying the same principles to modules can challenge existing thinking and practice.
  - Database schema per module

### Module boundaries are harder to keep closed (C#)

- In practice, it's often too easy for module boundaries in monoliths to be breached.
- Information hiding is hard in modules that are made of multiple assemblies
- The "internal" assemblies of the module need to expose key internal pieces via `public` modifiers.
- Most of these should not be accesible outside the boundary of the module
- However, due to the way packaging works in C#, it's possible for outside code to reference  these pieces.
- Ideally, outside code can only reference certain "use case" types from an "outside facing" assembly.

#### Work arounds

- One work around might be to just use one assembly
- However, this means that internal layers are now based on filesystem folders, instead of assemblies.
- Creates more opportunity for circular dependencies and loss of information hiding internally in the module itself.

# Communication

> The biggest issue in changing a monolith into microservices lies in changing the communication pattern. A naive conversion from in-memory method calls to RPC leads to chatty communications which don't perform well. Instead you need to replace the fine-grained communication with a coarser -grained approach.
>
> [Martin Fowler](https://martinfowler.com/articles/microservices.html)

## Technical mechanisms

### In-process

- Two software components running inside the same process.
- Running on the same platform / language.
- Both can communicate reliably together.
- Communication limitations are determined by the access controls available in the language.

### Intra-process

- Two software components running in separate processes
- These processes might be on the same machine.
  - Communication possibilities dependent on the OS
  - For example, XPC on macOS.
  - Generally communication can be considered very reliable
- They might also be on separate machines, potentially on different continents.
  - Communication has to occur over a network.
  - Reliability of communication is subject to various common networking challenges.
    - Poor connections
    - Unavailable services
    - Dropped packets
    - Latency
    - etc

## Workflows

![Microservices _ Modular Monoliths - Page 2](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - Page 2.png)

### Synchronous Communication (Blocking)

- **Component A** needs to communicate with **Component B**
- It has to wait until **Component B** is complete
  - Thus, **Component A** is blocked until **Component B** has completed.
- It may or may not need to return a data payload
  - Command or Query (CQRS)

### Asynchronous Communication (Non-Blocking)

- **Component A** needs to communicate with **Component B**
- **Component A** does not need to wait for **Component B** to complete
- **Component A** may or may not be expecting a response from **Component B**
  - **Component A** can therefore continue executing OR
  - **Component A** can wait until **Component B** has finished executing.
- **Component B** may take seconds, minutes, hours, days, years to complete

## Patterns

### Request-Response (Sync)

- A request is made from **Component A** to Component B
- **Component A** waits for a response from **Component B** before continue to execute
  - Connection to **Component B** remains open.
  - Resources used to execute **Component A** is in contention till the response is returned.
- Component A explicitly knows of **Component B**
- **Component B** may or may not know explicitly of **Component A**
- Example technologies
  - REST over HTTP
  - RPC

### Request-Response (Async)

- A request is made from **Component A** to Component B
- **Component A** waits for a response from **Component B** before continue to execute
  - No explicit "connection" remains open to **Component B**.
  - Resources used to execute **Component A** are freed whilst waiting.
  - Consider that multiple instances of **Component A** are available
    - A different instance of **Component A** may receive the response than the one that sent the request.
- **Component A** explicitly knows of **Component B**
- **Component B** may or may not know explicitly of **Component A**
- Example technologies
  - Queue-Based Brokers
  - RPC

### Event-Driven (Async)

- A system with 4 components, A, B, C and D
- **Component A** raises an "event" that something of interest has happened.
- **Component B** and **Component C** doens't care that this event happens.
  - They actually don't even know about it.
- **Component D** does care when this event happens.
  - It listens for the event and reacts accordingly when it occurs.
- None of the components need to know about each other.
- They only need to know what events can occur in the system, so they can react accordingly.
- Example technologies
  - Topic-Based Brokers
  - REST over HTTP i.e Atom

### Common Data (Async)

- **Component A** has new data, and places it in a CSV file on a shared drive.
- **Component B** periodically checks this shared drive for new data.
- Neither service needs to know anything about each other.
- Example technologies
  - File system
  - Database

### Mixing patterns

- A mix of these patterns can be used by a single Microservices
- Also, these patterns can be used both with in-process and intra-process mechanims.

## Communicating across boundaries

- Communication inside a system occurs across both
  - Logical boundaries
  - Technical boundaries

### Service Boundaries (Logical)

- Each service or module is defined by a boundary
- In DDD speak this is a "Bounded Context"
- This is a logic boundary that wraps a cohesive sub system of business functions

### Process Boundaries (Technical)

- This is a process boundary.
- Inside the boundary is a single deployable unit
  - In other words, an application
- In microservices, it wraps a single service
  - Therefore, a deployment unit is composed of a single services
- In modular monoliths, it can wrap one or more services.
  - Therefore, a deployment unit is composed of multiple services.
- Communications inside a 

### Comparison

- **Component A** and **Component B** represented as:
  - Modules in a modular monolith 
    - Communication occurs across multiple service boundaries
    - But it also occurse inside a single process boundary
  - Services in a distributed microservices setup
    - Communication occurs across multiple service boundaries
    - AND it also occurs across multiple process boundaries

![Microservices _ Modular Monoliths - Process Boundaries (1)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - Process Boundaries (1).png)

# Sharing Code

# Transactions

# Architecture

## Central Aggregating Gateway

### What is it

- Sits between individual microservices and clients that consume them.
- Simplifies the interface between them

### Benefits

- Decoupling of clients from individual "microservices". 
- Reduction of round trips from multiple services to build a workflow / screen
  - Client calls one interface
  - That interface aggregates calls to multiple internal interfaces (i.e other microservices)
  - Less latency
- Reduce chattiness between clients and services.
- Centralised security handling - "microservices" are free from having to be concerned with access level security 
- Other cross cutting handled.

### Challenges

- The gateway tier is coupled with the internal microservices. 
- Gateway becomes a single point of failure. 
- Single request to gateway, can result in multiple child requests to services, potential for long latency. 
- Gateway can become a bottleneck if not scaled properly. 
- As system grows, if gateway is owned by a single team, it can become a bottleneck for other teams who are changing services and need chagnes in the gateway.

![Microservices _ Modular Monoliths - CAG (MS)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - CAG (MS).png)

![Microservices _ Modular Monoliths - CAG (Monolith)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - CAG (Monolith).png)



## Backend For Frontend (BFF)

### What is it

- Could be considered an extension of Central Aggregating Gateway
- Creation of multiple built for purpose APIs that suit the requirements of individual clients.
- A useful approach when a One Size Fits All approach no longer works.

### Benefits

- More concise responses from services for particular clients (iOS vs Android)

### Challenges

- More APIs to handle
- Ownerships
  - Does the front end team own it? 
  - Do they have the skillset?
- See [Netflix approach](https://netflixtechblog.com/embracing-the-differences-inside-the-netflix-api-redesign-15fd8b3dc49d) to moving from One Size Fits All (OSFA)
- REA guy -  "one experience, one BFF."



![Microservices _ Modular Monoliths - BFF (MS)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - BFF (MS).png)

![Microservices _ Modular Monoliths - BFF (Monolith)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - BFF (Monolith).png)

## API Gateway

### What is it

- Acts very much like a reverse proxy

### Benefits

- Provides mechanisms such as
  - API Keys for external parties
  - Logging
  - Rate Limiting
  - Quotas
- Some gateways provide developer portals also.

### Challenges

- If you misuse it, then you can suffer:
  -  from vendor lock in
  - business logic making it's way into the gateway
- It becomes an intermediary for all microservice calls
  - Extra network hop

### Potential for misuse

- API Gateway's have been sold as a panacea to a number of problems that they shouldn't really be used for.
  - Sam Newman says this is due to the fact they were evolved in a time where the market for them seemed large
  - Turns out it wasn't, and now the VC funded ones are trying to find any kind of way to sell them into businesses.
- **Avoid doing aggregation**
  - Can lead to core business processes being baked into a third party tool
  - If you need to do aggregation, use the Central Aggregating Gateway or 
- **Avoid doing protocol rewriting**
  - i.e From XML to JSON
  - Pipes should be dumb, endpoints should be smart.
- [Smart endpoints and dumb pipes.](https://stackoverflow.com/questions/26616962/microservices-what-are-smart-endpoints-and-dumb-pipes)



![Microservices _ Modular Monoliths - API Gateway (MS)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - API Gateway (MS).png)

![Microservices _ Modular Monoliths - API Gateway (Monolith)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - API Gateway (Monolith).png)

# Versioning

WIP

## Versioning a modular monolith

- The monolithic application core itself is incorporate into API applications as if it was a library.
  - It's just an assembly after all.
- If we have multiple APIs running on top of the monolith, then there it could be so that we end up with multiple versions of the application core running at the same time, across different services. 

![Microservices _ Modular Monoliths - Multiple BFF #1](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - Multiple BFF #1.png)



# Resources

- [Building Microservices](https://samnewman.io/books/building_microservices_2nd_edition/) by Sam Newman 
