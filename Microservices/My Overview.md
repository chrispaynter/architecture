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

## Patterns

### Request-Response

- A request is made from **Component A** to Component B
- **Component A** waits for a response from **Component B** before continue to execute
- Component A explicitly knows of **Component B**
- **Component B** may or may not know explicitly of **Component A**

### Event-Driven

- A system with 4 components, A, B, C and D
- **Component A** raises an "event" that something of interest has happened.
- **Component B** and **Component C** doens't care that this event happens.
  - They actually don't even know about it.
- **Component D** does care when this event happens.
  - It listens for the event and reacts accordingly when it occurs.
- None of the components need to know about each other.
- They only need to know what events can occur in the system, so they can react accordingly.

### Common Data

# Sharing Code

# Transactions

# Versioning

# Architecture

