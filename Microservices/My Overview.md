# Microservices

[TOC]

# Boundaries

- What belongs inside a given microservice?
- What doesn't?

# Communication

## Technical considerations

### In-process Communication

- Two software components running inside the same process.
- Running on the same platform / language.
- Both can communicate reliably together.
- Communication limitations are determined by the access controls available in the language.

### Intra-process Communication

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

# Sharing Code

# Transactions

# Versioning

# Architecture

