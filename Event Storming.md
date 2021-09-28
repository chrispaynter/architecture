# Event Storming

## Resources

- Introducing Event Storming book
- [A facilitators recipe for Event Storming](https://medium.com/@springdo/a-facilitators-recipe-for-event-storming-941dcb38db0d)
- [Event Storming demo & discussion](https://www.youtube.com/watch?v=xIB_VQVVWKk)
- [Crunching 'real-life stories' with DDD & Event Storming - Kenny Baas-Schwegler - KanDDDinsky 2018](https://www.youtube.com/watch?v=WvkBKvMnyuc)
- [Trying out online EventStorming](https://www.youtube.com/watch?v=CbPEibNUe0s)
- [Big Picture Event Storming](https://medium.com/@chatuev/big-picture-event-storming-7a1fe18ffabb)

## The Event Storming Approach

Gather the best available brains for the job and collaboratively build a model of a very complex problem space.

- See the system as a whole.
- Find a problem/s worth solving.
- Gather the best immediately available information.
- Start implementing a solution from the best possible starting point.
- 

## Benefits

- Triggers conversations across organisational boundaries.
- Collaboration across multiple disciplines:
  - Business experts
  - Service Designers
  - Software Developers

## Possible formats of Event Storming

### Big Picture

- Good for kicking off a project.
- with every stakeholder involved.
- Avoid focusing too deeply on individual sections.
- Embrace the whole complexity and maximise learning.
- **Knowledge and/or learning is the most valuable outcome.**
- From "[Big Picture Event Storming](https://medium.com/@chatuev/big-picture-event-storming-7a1fe18ffabb)"
  - Following Vaughn Vernon, the core of Strategic Patterns is, **“Ubiquitous Language in an explicitly defined Bounded Context”**.
  - In my experience, in order to utilize DDD efficiently, it is better to start with domain discovery, knowledge sharing, and collecting the UL
  - In order to discover the domain in-depth and precisely collect the Ubiquitous Language, we need to create a shared pool of business-knowledge

### Design Level

- Digs into possible implementation
- Often DDD-CQRS/ES style
- Fewer, selected people.
- The focus is on implementing software features that are solving a specific problem.

There are others in the book.

# How to Event Storm

## Decide on boundaries

- Decide on the boundaries of the business domain you’re gonna discover. 
- On a Big picture event storming, you may tackle the whole business, 
  - or narrow down your focus to specific subdomains. 
- Understanding the scale will help you to define the invitee list.

## Preparation

![img](https://miro.medium.com/max/1400/1*UahWpqNOu0pdlcuyoq9anQ.jpeg)

## Time

- depends on the size of the subdomain you’re going to storm
- It’s very useful to run a day-long big-picture session for the first time in order to 
  - understand the scope of your domain, 
  - its’ complexity, and 
  - the most challenging parts
- For the future stormings, where you may focus on specific subdomains and discover particular problems
  - 2-3 hours-long sessions with a short break in the middle could be optimal.

## Getting Started

The scenario of the technique is to assess the current state of business first, and then to discover the most effective areas for improvements. So keep focus on the current state of the business domain.

To understand event storming let’s get familiar with the key artifacts:

### Domain Events

- any event that is important for your business and makes an explicit impact on it
- The event could happen inside or outside your business system. 
- The event could happen inside or outside your business system. 
- *Orange* sticky note



## Phases

### Phase: Chaotic Exploration

- Domain events (past tense) - `Product added to cart`
- Orange sticky note
- mostly relevant for domain experts

<img src="/Users/chrispaynter/Library/Application Support/typora-user-images/image-20210928201257680.png" alt="image-20210928201257680" style="zoom:30%; float:left;" />

### Phase: Enforcing the timeline

- Make sure enough space available
- Choose one or a combination of sorting strategies from below

<img src="/Users/chrispaynter/Library/Application Support/typora-user-images/image-20210928201352340.png" alt="image-20210928201352340" style="zoom:33%; float:left;" />

#### Pivotal Events

#### Swimlanes

#### Temporal Milestones

#### Chapters Sorting

