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

- A request is made from Compone
- A res

### Event-Driven

### Common Data

# Sharing Code

# Transactions

# Versioning

# Architecture

