# Chapter 1 - What are Microservices?

## Key Concepts of Microservices

### Independent Deployability

- Independent deployability is the idea that we can make a change to a microservice, deploy it, and release that change to our users, without having to deploy any other microservices. More important, it’s not just the fact that we can do this; it’s that this is actually how you manage deployments in your system. It’s a discipline you adopt as your default release approach. This is a simple idea that is nonetheless complex in execution.

### Modeled Around a Business Domain

- Techniques like domain-driven design can allow you to structure your code to better represent the real-world domain that the software operates in. With microservice architectures, we use this same idea to define our service boundaries. By modeling services around business domains, we can make it easier to roll out new functionality and to recombine microservices in different ways to deliver new functionality to our users.

### Owning Their Own State

- Hiding internal state in a microservice is analogous to the practice of encapsulation in object-oriented (OO) programming. Encapsulation of data in OO systems is an example of information hiding in action.
- Don’t share databases unless you really need to. And even then do everything you can to avoid it. In my opinion, sharing databases is one of the worst things you can do if you’re trying to achieve independent deployability

### Size

- James Lewis, technical director at Thoughtworks, has been known to say that “a microservice should be as big as my head.” At first glance, this doesn’t seem terribly helpful. After all, how big is James’s head exactly? The rationale behind this statement is that a microservice should be kept to the size at which it can be easily understood.

### Flexibility

- We don’t know what the future holds, so we’d like an architecture that can theoretically help us solve whatever problems we might face down the road. Finding a balance between keeping your options open and bearing the cost of architectures like this can be a real art.

### Alignment of Architecture and Organization

- Organizations which design systems...are constrained to produce designs which are copies of the communication structures of these organizations.
- A stream-aligned team is a team aligned to a single, valuable stream of work...[T]he team is empowered to build and deliver customer or user value as quickly, safely, and independently as possible, without requiring hand-offs to other teams to perform parts of the work.

## Enabling Technology

### Log Aggregation and Distributed Tracing

- Be cautious about taking on too much new technology when you start off with microservices. That said, a log aggregation tool is so essential that you should consider it a prerequisite for adopting microservices
- These systems allow you to collect and aggregate logs from across all your services, providing you a central place from which logs can be analyzed, and even made part of an active alerting mechanism

### Containers and Kubernetes

- Ideally, you want to run each microservice instance in isolation. This ensures that issues in one microservice can’t affect another microservice—for example, by gobbling up all the CPU.
- After you begin playing around with containers, you’ll also realize that you need something to allow you to manage these containers across lots of underlying machines. Container orchestration platforms like Kubernetes do exactly that, allowing you to distribute container instances in such a way as to provide the robustness and throughput your service needs, while allowing you to make efficient use of the underlying machines

### Streaming

- Although with microservices we are moving away from monolithic databases, we still need to find ways to share data between microservices.
- For many people, Apache Kafka has become the de facto choice for streaming data in a microservice environment, and for good reason. Capabilities such as message permanence, compaction, and the ability to scale to handle large volumes of messages can be incredibly useful.

### Public Cloud and Serverless

- As your microservice architecture grows, more and more work will be pushed into the operational space. Public cloud providers offer a host of managed services, from managed database instances or Kubernetes clusters to message brokers or distributed filesystems. By making use of these managed services, you are offloading a large amount of this work to a third party that is arguably better able to deal with these tasks.

## Advantages of Microservices

### Technology Heterogeneity

- With a system composed of multiple, collaborating microservices, we can decide to use different technologies inside each one. This allows us to pick the right tool for each job rather than having to select a more standardized, one-size-fits-all approach that often ends up being the lowest common denominator.
- With microservices, we are also able to more quickly adopt technologies and to understand how new advancements might help us. One of the biggest barriers to trying out and adopting a new technology is the risks associated with it.

### Robustness

- A key concept in improving the robustness of your application is the bulkhead. A component of a system may fail, but as long as that failure doesn’t cascade, you can isolate the problem, and the rest of the system can carry on working. Service boundaries become your obvious bulkheads. In a monolithic service, if the service fails, everything stops working. With a monolithic system, we can run on multiple machines to reduce our chance of failure, but with microservices, we can build systems that handle the total failure of some of the constituent services and degrade functionality accordingly.
- Networks can and will fail, as will machines. We need to know how to handle such failures and the impact (if any) those failures will have on the end users of our software.

### Scaling

- With a large, monolithic service, we need to scale everything together. Perhaps one small part of our overall system is constrained in performance, but if that behavior is locked up in a giant monolithic application, we need to handle scaling everything as a piece.
- With smaller services, we can scale just those services that need scaling, allowing us to run other parts of the system on smaller, less powerful hardware

### Ease of Deployment

- A one-line change to a million-line monolithic application requires the entire application to be deployed in order to release the change. That could be a large-impact, high-risk deployment. In practice, deployments such as these end up happening infrequently because of understandable fear.
- With microservices, we can make a change to a single service and deploy it independently of the rest of the system. This allows us to get our code deployed more quickly. If a problem does occur, it can be quickly isolated to an individual service, making fast rollback easy to achieve.
- This is one of the main reasons organizations like Amazon and Netflix use these architectures—to ensure that they remove as many impediments as possible to getting software out the door.

### Organizational Alignment

- Microservices allow us to better align our architecture to our organization, helping us minimize the number of people working on any one codebase to hit the sweet spot of team size and productivity. Microservices also allow us to change ownership of services as the organization changes—enabling us to maintain the alignment between architecture and organization in the future.

### Composability

- With microservices, we allow for our functionality to be consumed in different ways for different purposes
- With microservices, think of us opening up seams in our system that are addressable by outside parties.

## Microservice Pain Points

### Developer Experience

- As you have more and more services, the developer experience can begin to suffer. More resource-intensive runtimes like the JVM can limit the number of microservices that can be run on a single developer machine.

### Technology Overload
 
- There is a danger, though, that this wealth of new toys can lead to a form of technology fetishism. I’ve seen so many companies adopting microservice architecture who decided that it was also the best time to introduce vast arrays of new and often alien technology.
- You have to carefully balance the breadth and complexity of the technology you use against the costs that a diverse array of technology can bring.
- As you (gradually) increase the complexity of your microservice architecture, look to introduce new technology as you need it. You don’t need a Kubernetes cluster when you have three services! In addition to ensuring that you’re not overloaded with the complexity of these new tools, this gradual increase has the added benefit of allowing you to gain new and better ways of doing things that will no doubt emerge over time.

### Cost

- In my experience, microservices are a poor choice for an organization primarily concerned with reducing costs, as a cost-cutting mentality—where IT is seen as a cost center rather than a profit center—will constantly be a drag on getting the most out of this architecture. On the other hand, microservices can help you make more money if you can use these architectures to reach more customers or develop more functionality in parallel. So are microservices a way to drive more profits? Perhaps. Are microservices a way to reduce costs? Not so much.

### Reporting

- With a microservice architecture, we have broken up this monolithic schema. That doesn’t mean that the need for reporting across all our data has gone away; we’ve just made it much more difficult, because now our data is scattered across multiple logically isolated schemas.

### Monitoring and Troubleshooting

- With a standard monolithic application, we can have a fairly simplistic approach to monitoring. We have a small number of machines to worry about, and the failure mode of the application is somewhat binary—the application is often either all up or all down. With a microservice architecture, do we understand the impact if just a single instance of a service goes down?
- With a monolithic system, if our CPU is stuck at 100% for a long time, we know it’s a big problem. With a microservice architecture with tens or hundreds of processes, can we say the same thing? Do we need to wake someone up at 3 a.m. when just one process is stuck at 100% CPU?

### Security

- With a single-process monolithic system, much of our information flowed within that process. Now, more information flows over networks between our services. This can make our data more vulnerable to being observed in transit and also to potentially being manipulated as part of man-in-the-middle attacks. This means that you might need to direct more care to protecting data in transit and to ensuring that your microservice endpoints are protected so that only authorized parties are able to make use of them.

### Testing

- End-to-end tests for any type of system are at the extreme end of the scale in terms of the functionality they cover, and we are used to them being more problematic to write and maintain than smaller-scoped unit tests. Often this is worth it, though, because we want the confidence that comes from having an end-to-end test use our systems in the same way a user might. But with a microservice architecture, the scope of our end-to-end tests becomes very large. We would now need to run tests across multiple processes, all of which need to be deployed and appropriately configured for the test scenarios. We also need to be prepared for the false negatives that occur when environmental issues, such as service instances dying or network time-outs of failed deployments, cause our tests to fail.
- The testing will cost more but won’t manage to give you the same level of confidence that it did in the past. This will drive you toward new forms of testing, such as contract-driven testing or testing in production, as well as the exploration of progressive delivery techniques such as parallel runs or canary releases

### Latency

- With a microservice architecture, processing that might previously have been done locally on one processor can now end up being split across multiple separate microservices. Information that previously flowed within only a single process now needs to be serialized, transmitted, and deserialized over networks that you might be exercising more than ever before.
- Although it can be difficult to measure the exact impact on latency of operations at the design or coding phase, this is another reason it’s important to undertake any microservice migration in an incremental fashion.
- But you also need to have an understanding of what latency is acceptable for these operations. Sometimes making an operation slower is perfectly acceptable, as long as it is still fast enough!

### Data Consistency

- Whereas in the past you might have relied on database transactions to manage state changes, you’ll need to understand that similar safety cannot easily be provided in a distributed system. The use of distributed transactions in most cases proves to be highly problematic in coordinating state changes.
- Instead, you might need to start using concepts like sagas and eventual consistency to manage and reason about state in your system.
- Adopting an incremental approach to decomposition, so that you are able to assess the impact of changes to your architecture in production, is really important.

## Should I Use Microservices?

### Whom They Might Not Work For

- Given the importance of defining stable service boundaries, I feel that microservice architectures are often a bad choice for brand-new products or startups. In general, I feel it’s more appropriate to wait until enough of the domain model has stabilized before looking to define service boundaries.
- The process of finding product market fit means that you might end up with a very different product at the end than the one you thought you’d build when you started.
- Startups also typically have fewer people available to build the system, which creates more challenges with respect to microservices. Microservices bring with them sources of new work and complexity, and this can tie up valuable bandwidth. The smaller the team, the more pronounced this cost will be.
- For a small team, a microservice architecture can be difficult to justify because there is work required just to handle the deployment and management of the microservices themselves. Some people have described this as the “microservice tax.”
- Finally, organizations creating software that will be deployed and managed by their customers may struggle with microservices.

### Where They Work Well

- In my experience, probably the single biggest reason that organizations adopt microservices is to allow for more developers to work on the same system without getting in each other’s way.
- A five-person startup is likely to find a microservice architecture a drag. A hundred-person scale-up that is growing rapidly is likely to find that its growth is much easier to accommodate with a microservice architecture properly aligned around its product development efforts.
- The technology-agnostic nature of microservices ensures that you can get the most out of cloud platforms.
- Microservices also present clear benefits for organizations looking to provide services to their customers over a variety of new channels. A lot of digital transformation efforts seem to involve trying to unlock functionality hidden away in existing systems. The desire is to create new customer experiences that can support the needs of users via whatever interaction mechanism makes the most sense.
- Above all, a microservice architecture is one that can give you a lot of flexibility as you continue to evolve your system. That flexibility has a cost, of course, but if you want to keep your options open regarding changes you might want to make in the future, it could be a price worth paying.

## Summary

- Microservice architectures can give you a huge degree of flexibility in choosing technology, handling robustness and scaling, organizing teams, and more. This flexibility is in part why many people are embracing microservice architectures. But microservices bring with them a significant degree of complexity, and you need to ensure that this complexity is warranted.