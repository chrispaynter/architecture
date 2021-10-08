# Microservices and Modular Monoliths

This is an attempt to aggregate information about both architectural styles in a way that can be referenced as a guide during architectural thinking.

[TOC]

# Boundaries

- What belongs inside a given microservice?
- What doesn't?

## Boundaries in a modular monolith

> Design as microservices, build as modules.

- We can apply the same approach to designing modules as we do with microservices.
- After all, in DDD speak, they're esentially bounded contexts.
- However, applying the same principles to modules can challenge existing thinking and practice.

# Communication

## Resources

- [Building Microservices](https://samnewman.io/books/building_microservices_2nd_edition/) by Sam Newman

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

# Versioning

# Architecture

## Central Aggregating Gateway

### What is it

- Sits between individual microservices and clients that consume them.
- Simplifies the interface between them

### Benefits

- Decoupling of clients from individual "microservices". 
- Reduction of round trips from multiple services to build a workflow / screen 
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
- 



![Microservices _ Modular Monoliths - BFF (MS)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - BFF (MS).png)

![Microservices _ Modular Monoliths - BFF (Monolith)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - BFF (Monolith).png)

## API Gateway

### What is it

- Could be considered an extension of Central Aggregating Gateway

![Microservices _ Modular Monoliths - API Gateway (MS)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - API Gateway (MS).png)

![Microservices _ Modular Monoliths - API Gateway (Monolith)](/Users/chrispaynter/Downloads/Microservices _ Modular Monoliths - API Gateway (Monolith).png)
