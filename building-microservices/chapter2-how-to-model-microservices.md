# Chapter 2 - How to Model Microservices?

## What Makes a Good Microservice Boundary?

#### Information Hiding

- Information hiding describes a desire to hide as many details as possible behind a module (or, in our case, microservice) boundary
  - Improved development time
    - By allowing modules to be developed independently, we can allow for more work to be done in parallel and reduce the impact of adding more developers to a project.
  - Comprehensibility
    - Each module can be looked at in isolation and understood in isolation. This in turn makes it easier to understand what the system as a whole does.
  - Flexibility
    - Modules can be changed independently from one another, allowing for changes to be made to the functionality of the system without requiring other modules to change. In addition, modules can be combined in different ways to deliver new functionality.
- Having modules does not result in your actually achieving these outcomes. A lot depends on how the module boundaries are formed.
- _The connections between modules are the assumptions which the modules make about each other._ - Parnas

#### Cohesion

- One of the most succinct definitions I’ve heard for describing cohesion is this: _“the code that changes together, stays together.”_
- So we want to find boundaries within our problem domain that help ensure related behavior is in one place and that communicate with other boundaries as loosely as possible.

#### Coupling

- When services are loosely coupled, a change to one service should not require a change to another. The whole point of a microservice is being able to make a change to one service and deploy it without needing to change any other part of the system. This is really quite important.

#### The Interplay of Coupling and Cohesion

- _A structure is stable if cohesion is strong and coupling is low_ - Constantin's Law, Larry Constantine
- Cohesion applies to the relationship between things inside a boundary (a microservice in our context), whereas coupling describes the relationship between things across a boundary.

## Types of Coupling

#### Domain Coupling

- *__Domain coupling__* describes a situation in which one microservice needs to interact with another microservice, because the first microservice needs to make use of the functionality that the other microservice provides.
- As a general rule, domain coupling is considered to be a loose form of coupling, although even here we can hit problems. A microservice that needs to talk to lots of downstream microservices might point to a situation in which too much logic has been centralized.
- Just remember the importance of information hiding. Share only what you absolutely have to, and send only the absolute minimum amount of data that you need.
- *__Temporal coupling__* has a subtly different meaning in the context of a distributed system, where it refers to a situation in which one microservice needs another microservice to do something at the same time for the operation to complete.
- Temporal coupling isn’t always bad; it’s just something to be aware of. One of the ways to avoid temporal coupling is to use some form of asynchronous communication, such as a message broker.

#### Pass-Through Coupling

- *__Pass-through coupling__* describes a situation in which one microservice passes data to another microservice purely because the data is needed by some other microservice further downstream
- The major issue with pass-through coupling is that a change to the required data downstream can cause a more significant upstream change.

#### Common Coupling

- *__Common coupling__* occurs when two or more microservices make use of a common set of data. A simple and common example of this form of coupling would be multiple microservices making use of the same shared database, but it could also manifest itself in the use of shared memory or a shared filesystem.
- The main issue with common coupling is that changes to the structure of the data can impact multiple microservices at once.
- _Multiple services accessing shared static reference data related to countries from the same database_ is, relatively speaking, fairly benign. This is because by its very nature static reference data doesn’t tend to change often, and also because this data is read-only—as a result I tend to be relaxed about sharing static reference data in this way.
- An example of common coupling in which both <code>Order Processor Service</code> and <code>Warehouse Service</code> are updating the same <code>Order</code> record on a shared database.
- A potential solution here would be to ensure that a single microservice manages the order state. Either <code>Warehouse Service</code> or <code>Order Processor Service</code> can send status update requests to the <code>Order Service</code>. Here, the <code>Order Service</code> is the source of truth for any given order.
- _Make sure you see a request that is sent to a microservice as something that the downstream microservice can reject if it is invalid._
- *__WARNING__* If you see a microservice that just looks like a thin wrapper around database CRUD operations, that is a sign that you may have weak cohesion and tighter coupling, as logic that should be in that service to manage the data is instead spread elsewhere in your system.

#### Content Coupling

- *__Content coupling__* describes a situation in which an upstream service reaches into the internals of a downstream service and changes its internal state. The most common manifestation of this is an external service accessing another microservice’s database and changing it directly.
- If you are working on a microservice, it’s vital that you have a clear separation between what can be changed freely and what cannot. 
- You need to ensure that if you make changes, that you will not break upstream consumers. Functionality that doesn’t impact the contract your microservice exposes can be changed without concern.
- It’s certainly the case that the problems that occur with common coupling also apply with content coupling, but content coupling has some additional headaches that make it problematic enough that some people refer to it as _pathological coupling_.
- __IN SHORT JUST AVOID CONTENT COUPLING__.

## Just Enough Domain-Driven Design

#### Ubiquitous Language

- Ubiquitous language refers to the idea that we should strive to use the same terms in our code as the users use. The idea is that having a common language between the delivery team and the actual people will make it easier to model the real-world domain and also should improve communication.
- By working the real-world language into the code, things became much easier. A developer picking up a story written using the terms that had come straight from the product owner was much more likely to understand their meaning and work out what needed to be done.

#### Aggregate

- Aggregates typically have a life cycle around them, which opens them up to being implemented as a state machine.
- We want to treat aggregates as self-contained units; we want to ensure that the code that handles the state transitions of an aggregate are grouped together, along with the state itself. So one aggregate should be managed by one microservice, although a single microservice might own management of multiple aggregates.
- In general, though, you should think of an aggregate as something that has state, identity, a life cycle that will be managed as part of the system. Aggregates typically refer to real-world concepts.
- The key thing to understand here is that if an outside party requests a state transition in an aggregate, the aggregate can say no. You ideally want to implement your aggregates in such a way that illegal state transitions are impossible.

#### Bounded Context

- A bounded context typically represents a larger organizational boundary. Within the scope of that boundary, explicit responsibilities need to be carried out.
- Bounded contexts hide implementation details.
- From an implementation point of view, bounded contexts contain one or more aggregates. Some aggregates may be exposed outside the bounded context; others may be hidden internally. As with aggregates, bounded contexts may have relationships with other bounded contexts—when mapped to services, these dependencies become inter-service dependencies.

  ###### Hidden models

  <p>
  For MusicCorp, we can consider the finance department and the warehouse to be two separate bounded contexts. The finance department does not need to know about the detailed inner workings of the warehouse. 
  It does need to know some things, however; for example, it needs to know about stock levels to keep the accounts up to date.
  To be able to work out the valuation of the company, though, the finance employees need information about the stock we hold. The stock item then becomes a shared model between the two contexts.
  Stock Item inside the warehouse bounded context contains references to the shelf locations, but the shared representation contains only a count. So there is the internal-only representation and the external representation we expose.
  </p>

  ###### Shared models

  <p>
  When you have a situation like this, a shared model like stock item can have different meanings in the different bounded contexts and therefore might be called different things. 
  We might be happy to keep the name “stock item” in warehouse, but in finance we might refer to it more generically as an “asset,” as that is the role they play in that context. 
  We store information about the stock item in both locations, but the information is different.
  </p>

#### Mapping Aggregates and Bounded Contexts to Microservices

- The aggregate is a self-contained state machine that focuses on a single domain concept in our system, with the bounded context representing a collection of associated aggregates, again with an explicit interface to the wider world.
  ###### Turtles all the way down
  At the start, you will probably identify a number of coarse-grained bounded contexts. But these bounded contexts can in turn contain further bounded contexts. For example, you could decompose the warehouse into capabilities associated with order fulfillment, inventory management, or goods receiving. When considering the boundaries of your microservices, first think in terms of the larger, coarser-grained contexts, and then subdivide along these nested contexts when you’re looking for the benefits of splitting out these seams.
  <br> A trick here is that even if you decide to split a service that models an entire bounded context into smaller services later on, you can still hide this decision from the outside world—perhaps by presenting a coarser-grained API to consumers.

#### Event Storming

- Event storming, a technique developed by Alberto Brandolini, is a collaborative brainstorming exercise designed to help surface a domain model.
  ###### Logistics
  ###### The Process
  - events, commands ,aggregates

## The Case for Domain-Driven Design for Microservices

- Firstly, a big part of what makes DDD so powerful is that bounded contexts, which are so important to DDD, are explicitly about hiding information—presenting a clear boundary to the wider system while hiding internal complexity that is able to change without impacting other parts of the system.
- Secondly, the focus on defining a common, ubiquitous language helps greatly when it comes to defining microservice endpoints.
- Fundamentally, DDD puts the business domain at the heart of the software we are building. The encouragement that it gives us to pull the language of the business into our code and service design helps improve domain expertise among the people who build the software.

## Alternatives to Business Domain Boundaries

#### Volatility

- Volatility-based decomposition has you identify the parts of your system going through more frequent change and then extract that functionality into their own services, where they can be more effectively worked on. Conceptually, I don’t have a problem with this, but promoting it as the only way to do things isn’t helpful, especially when we consider the different drivers that might be pushing us toward microservices. If my biggest issue is related to the need to scale my application, for example, a volatility-based decomposition is unlikely to deliver much of a benefit.

#### Data

- The nature of the data you hold and manage can drive you toward different forms of decomposition. For example, you might want to limit which services handle personally identifiable information (PII), both to reduce your risk of data breaches and to simplify oversight and implementation of things like GDPR.
- Segregation of data is often driven by a variety of privacy and security concerns

#### Technology

- The need to make use of different technology can also be a factor in terms of finding a boundary

#### Organizational

- How you organize yourself ends up driving your systems architecture, for good or for ill. When it comes to helping us define our service boundaries, we have to consider this as a key part of our decision making.

## Mixing Models and Exceptions

- As I hope is clear so far, I am not dogmatic in terms of how you find these boundaries. If you follow the guidelines of information hiding and appreciate the interplay of coupling and cohesion, then chances are you’ll avoid some of the worst pitfalls of whatever mechanism you pick.

## Summary

- In this chapter, you’ve learned a bit about what makes a good microservice boundary, and how to find seams in our problem space that give us the dual benefits of both low coupling and strong cohesion. 
  Having a detailed understanding of our domain can be a vital tool in helping us find these seams, and by aligning our microservices to these boundaries we ensure that the resulting system has every chance of keeping those virtues intact.
