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

