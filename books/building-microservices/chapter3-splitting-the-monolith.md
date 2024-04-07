# Chapter 3 - Splitting the Monolith

## Have a Goal

- Microservices are not the goal. You don’t “win” by having microservices.
- Fixating on microservices rather than on the end goal also means you will likely stop thinking of other ways in which you might bring about the change you are looking for. For example, microservices can help you scale your system, but there are often a number of alternative scaling techniques that should be looked at first.
- Microservices aren’t easy. Try the simple stuff first.

## Incremental Migration

- _If you do a big-bang rewrite, the only thing you’re guaranteed of is a big bang._ - Martin Fowler
- An incremental approach will help you learn about microservices as you go and will also limit the impact of getting something wrong (and you will get things wrong!). 
- By splitting out microservices one at a time, you also get to unlock the value they bring incrementally, rather than having to wait for some big bang deployment.

## The Monolith Is Rarely the Enemy

- Don’t focus on “not having the monolith”; focus instead on the benefits you expect your change in architecture to bring.
- Many people find the reality of a monolith and microservices coexisting to be “messy”—but the architecture of a real-world running system is never clean or pristine.
- Real system architecture is a constantly evolving thing that must adapt as needs and knowledge change. The skill is in getting used to this idea.

#### The Dangers of Premature Decomposition

- Prematurely decomposing a system into microservices can be costly, especially if you are new to the domain. In many ways, having an existing codebase you want to decompose into microservices is much easier than trying to go to microservices from the beginning for this very reason.

## What to Split First?

- Fundamentally, the decision about which functionality to split into a microservice will end up being a balance between these two forces—how easy the extraction is versus the benefit of extracting the microservice in the first place.
- My advice for the first couple of microservices would be to pick things that lean a bit more toward the “easy” end of the spectrum—a microservice that we think has some impact in terms of achieving our end-to-end goal, certainly, but something we’d consider to be low-hanging fruit.

## Decomposition by Layer

- Often decomposition of the UI tends to lag behind decomposition of the backend into microservices, since until the microservices are available, it’s difficult to see the possibilities for UI decomposition; just make sure it doesn’t lag too much.
- There is some application code that lives in the monolith, and some related data storage in the database. So which bit should we extract first?

#### Code First

- Extracting the application code tends to be easier than extracting things from the database. If we found that it was impossible to extract the application code cleanly, we could abort any further work, avoiding the need to detangle the database.
- Do the legwork to sketch out how both application code and data will be extracted before you start.

#### Data First

- I see this approach less often, but it can be useful in situations in which you are unsure whether the data can be separated cleanly.
- The main benefit of this approach in the short term is in derisking the full extraction of the microservice. It forces you to deal up front with issues like loss of enforced data integrity in your database or lack of transactional operations across both sets of data.

## Useful Decompositional Patterns

#### Strangler Fig Pattern

- A technique that has seen frequent use during system rewrites is the strangler fig pattern, a term coined by Martin Fowler. Inspired by a type of plant, the pattern describes the process of wrapping an old system with the new system over time, allowing the new system to take over more and more features of the old system incrementally.
- The beauty of this pattern is that it can often be done without making any changes to the underlying monolithic application. The monolith is unaware that it has even been “wrapped” with a newer system.

#### Parallel Run

- One way to make sure the new functionality is working well without risking the existing system behavior is to make use of the parallel run pattern: running both your monolithic implementation of the functionality and the new microservice implementation side by side, serving the same requests, and comparing the results.

#### Feature Toggle

- A feature toggle is a mechanism that allows a feature to be switched off or on, or to switch between two different implementations of some functionality.
- With the strangler fig pattern example of using an HTTP proxy, we could implement the feature toggle in the proxy layer to allow for a simple control to switch between implementations.

## Data Decomposition Concerns

#### Performance

- Databases, especially relational databases, are good at joining data across different tables. Very good. So good, in fact, that we take this for granted. Often, though, when we split databases apart in the name of microservices, we end up having to move join operations from the data tier up into the microservices themselves. And try as we might, it’s unlikely to be as fast.
- Logically, the join operation is still happening, but it is now happening inside the Finance microservice rather than in the database. The join has gone from the data tier to the application code tier.
- In this situation, I’d be very surprised if the overall latency of this operation didn’t increase. That may not be a significant problem in this particular case, as this report is generated monthly and could therefore be aggressively cached.

#### Data Integrity

- With both the Album and Ledger tables being in the same database, we could (and likely would) define a foreign key relationship between the rows in the Ledger table and the Album table.
- With these tables now living in different databases, we no longer have enforcement of the integrity of our data model. There is nothing to stop us from deleting a row in the Album table, causing an issue when we try to work out exactly what item was sold.
- To an extent, you’ll simply need to get used to the fact that you can no longer rely on your database to enforce the integrity of inter-entity relationships.

#### Transactions

- Once we start splitting data across multiple databases, though, we lose the safety of the ACID transactions we are used to.
- For people moving from a system in which all state changes could be managed in a single transactional boundary, the shift to distributed systems can be a shock, and often the reaction is to look to implement distributed transactions to regain the guarantees that ACID transactions gave us with simpler architectures.
- Distributed transactions are not only complex to implement, even when done well, but they also don’t actually give us the same guarantees that we came to expect with more narrowly scoped database transactions.

#### Tooling

- Changing databases is difficult for many reasons, one of which is that limited tools remain available to allow us to make changes easily.

#### Reporting Database

- As part of extracting microservices from our monolithic application, we also break apart our databases, as we want to hide access to our internal data storage.
- With a reporting database, we instead create a dedicated database that is designed for external access, and we make it the responsibility of the microservice to push data from internal storage to the externally accessible reporting database
- The reporting database allows us to hide internal state management, while still presenting the data in a database—something which can be very useful.
- There are two key points to highlight here. 
  - Firstly, we still want to practice information hiding.  So we should expose only the bare minimum of data in the reporting database.
  - The second key point is that the reporting database should be treated like any other microservice endpoint, and it is the job of the microservice maintainer to ensure that compatibility of this endpoint is maintained even if the microservice changes its internal implementation detail. The mapping from internal state to reporting database is the responsibility of the people who develop the microservice itself.

## Summary

- when embarking on work to migrate functionality from a monolithic architecture to a microservice architecture, you must have a clear understanding of what you expect to achieve.
- Migration should be incremental. Make a change, roll that change out, assess it, and go again.


