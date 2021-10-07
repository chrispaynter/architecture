# Modular Monolith

[TOC]

## Resources

- [Modular Monolith: A Primer](http://www.kamilgrzybek.com/design/modular-monolith-primer/)
- https://github.com/chrispaynter/modular-monolith-with-ddd (Forked)
- [Modular Monoliths • Simon Brown • GOTO 2018](https://www.youtube.com/watch?v=5OjqD-ow8GE)
- [Majestic Modular Monoliths by Axel Fontaine](https://www.youtube.com/watch?v=BOvxJaklcr0)
- [START with a Monolith, NOT Microservices](https://www.youtube.com/watch?v=Z_pj1mUDKdw)
- [Modular Monoliths With ASP.NET Core - Pragmatic Architecture](https://www.thinktecture.com/en/asp-net/modular-monolith/)
- [Modules or Microservices? - Sander Mak](https://www.youtube.com/watch?v=AJW2FAJGgVw)

## Comparison

<img src="/Users/chrispaynter/Library/Application Support/typora-user-images/image-20211007090206655.png" alt="image-20211007090206655" style="zoom:50%;" />

## Benifits of Modules

- Ease of deployment and management
- Strong but refactorable boundaries
- Strongly typed, in process communication
  - No serialization or network latency
  - Synchronous communication
- Eventual consistency is a choice
  - It's still good to partition data
- Explicit dependencies
  - 

## Modular Monolith vs. Microservices

- Monoliths have one deployment unit
  - Microservices can be deployed independently of each other.
- The "single deployment" concept has bigger consequences than deployment speed alone
  - limits the capacity for disposability – a key benefit of microservices.
  - imagine a microservices application that receives a petabyte of data it needs to process. 
  - It receives the data, spins up as many services needed to handle that data
  - then disposes of those services once the data has been processed.
  - [What is a Modular Monolith?](https://www.jrebel.com/blog/what-is-a-modular-monolith)

![image-20210930133353076](/Users/chrispaynter/Library/Application Support/typora-user-images/image-20210930133353076.png)

![image-20210930133532689](/Users/chrispaynter/Library/Application Support/typora-user-images/image-20210930133532689.png)

![image-20210930134841528](/Users/chrispaynter/Library/Application Support/typora-user-images/image-20210930134841528.png)

![image-20210930135655247](/Users/chrispaynter/Library/Application Support/typora-user-images/image-20210930135655247.png)

![image-20210930135932221](/Users/chrispaynter/Library/Application Support/typora-user-images/image-20210930135932221.png)

