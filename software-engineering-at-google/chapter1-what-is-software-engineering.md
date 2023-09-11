# Chapter 1. What Is Software Engineering?

## Time and Change

- Getting through not only that first big upgrade, but getting to the point at which you can reliably stay current going forward, is the essence of long-term sustainability for your project. Sustainability requires planning and managing the impact of required change. For many projects at Google, we believe we have achieved this sort of sustainability, largely through trial and error.
- keeping software maintainable for the long-term is a constant battle

#### Hyrum’s Law

- If you are maintaining a project that is used by other engineers, the most important lesson about “it works” versus “it is maintainable” is what we’ve come to call Hyrum’s Law:
_With a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviors of your system will be depended on by somebody._
- It is conceptually akin to entropy: discussions of change and maintenance over time must be aware of Hyrum’s Law just as discussions of efficiency or thermodynamics must be mindful of entropy.
- Hyrum’s Law represents the practical knowledge that—even with the best of intentions, the best engineers, and solid practices for code review—we cannot assume perfect adherence to published contracts or best practices. As an API owner, you will gain some flexibility and freedom by being clear about interface promises, but in practice, the complexity and difficulty of a given change also depends on how useful a user finds some observable behavior of your API.

#### Example: Hash Ordering

- Most programmers know that hash tables are non-obviously ordered. Few know the specifics of whether the particular hash table they are using is intending to provide that particular ordering forever.
- As a result, if you ask any expert “Can I assume a particular output sequence for my hash container?” that expert will presumably say “No.” By and large that is correct, but perhaps simplistic. A more nuanced answer is, “If your code is short-lived, with no changes to your hardware, language runtime, or choice of data structure, such an assumption is fine. If you don’t know how long your code will live, or you cannot promise that nothing you depend upon will ever change, such an assumption is incorrect.”
- Some languages specifically randomize hash ordering between library versions or even between execution of the same program in an attempt to prevent dependencies. But even this still allows for some Hyrum’s Law surprises: there is code that uses hash iteration ordering as an inefficient random-number generator. Removing such randomness now would break those users. Just as entropy increases in every thermodynamic system, Hyrum’s Law applies to every observable behavior.
- differences between code written with a “works now” and a “works indefinitely” mentality
- We’ve taken to saying, “It’s programming if ‘clever’ is a compliment, but it’s software engineering if ‘clever’ is an accusation.”

#### Why Not Just Aim for “Nothing Changes”?

- Every piece of technology upon which your project depends has some (hopefully small) risk of containing critical bugs and security vulnerabilities that might come to light only after you’ve started relying on it.
- Change is not inherently good. We shouldn’t change just for the sake of change. But we do need to be capable of change.

## Scale and Efficiency

- Many questions, such as “How long does it take to do a full build?”, “How long does it take to pull a fresh copy of the repository?”, or “How much will it cost to upgrade to a new language version?” aren’t actively monitored and change at a slow pace. They can easily become like the metaphorical boiled frog; it is far too easy for problems to worsen slowly and never manifest as a singular moment of crisis.
- The boiling frog story is generally offered as a metaphor cautioning people to be aware of even gradual change lest they suffer eventual undesirable consequences.

#### Policies That Don’t Scale

- We’ve also learned that having a dedicated group of experts execute the change scales better than asking for more maintenance effort from every user: experts spend some time learning the whole problem in depth and then apply that expertise to every subproblem. Forcing users to respond to churn means that every affected team does a worse job ramping up, solves their immediate problem, and then throws away that now-useless knowledge. Expertise scales better.

#### Policies That Scale Well

- More colloquially, this is phrased as “If you liked it, you should have put a CI test on it,” which we call “The Beyoncé Rule.”

#### Example: Compiler Upgrade

- Through this and other experiences, we’ve discovered many factors that affect the flexibility of a codebase:
    ###### Expertise
    - We know how to do this; for some languages, we’ve now done hundreds of compiler upgrades across many platforms.
    ###### Stability
    - There is less change between releases because we adopt releases more regularly; for some languages, we’re now deploying compiler upgrades every week or two.
    ###### Conformity
    - There is less code that hasn’t been through an upgrade already, again because we are upgrading regularly.
    ###### Familiarity
    - Because we do this regularly enough, we can spot redundancies in the process of performing an upgrade and attempt to automate. This overlaps significantly with SRE views on toil.
    ###### Policy
    - We have processes and policies like the Beyoncé Rule. The net effect of these processes is that upgrades remain feasible because infrastructure teams do not need to worry about every unknown usage, only the ones that are visible in our CI systems.

#### Shifting Left

- One of the broad truths we’ve seen to be true is the idea that finding problems earlier in the developer workflow usually reduces costs.
- The argument in this case is relatively simple: if a security problem is discovered only after your product has gone to production, you have a very expensive problem. If it is caught before deploying to production, it may still take a lot of work to identify and remedy the problem, but it’s cheaper.

## Trade-offs and Costs

- Within Google, there is a strong distaste for “because I said so.” It is important for there to be a decider for any topic and clear escalation paths when decisions seem to be wrong, but the goal is consensus, not unanimity. It’s fine and expected to see some instances of “I don’t agree with your metrics/valuation, but I see how you can come to that conclusion.”

#### Example: Markers

- Making good engineering decisions is all about weighing all of the available inputs and making informed decisions about the trade-offs
- In the end, decisions in an engineering group should come down to very few things:

    - We are doing this because we must (legal requirements, customer requirements).
    - We are doing this because it is the best option (as determined by some appropriate decider) we can see at the time, based on current evidence.
- Decisions should not be “We are doing this because I said so.”

#### Inputs to Decision Making

- All of the quantities involved are measurable or can at least be estimated.
- Some of the quantities are subtle, or we don’t know how to measure them. We rely on experience, leadership, and precedent to negotiate these issues.

#### Example: Distributed Builds

- how much productive time in your organization is lost waiting for a build?
- Jevons Paradox: consumption of a resource may increase as a response to greater efficiency in its use.

#### Example: Deciding Between Time and Scale

- Occasionally time and scale come into conflict, and nowhere so clearly as in the basic question: should we add a dependency or fork/reimplement it to better suit our local needs?
- On the other hand, if every developer forks everything used in their software project instead of reusing what exists, scalability suffers alongside sustainability. Reacting to a security issue in an underlying library is no longer a matter of updating a single dependency and its users: it is now a matter of identifying every vulnerable fork of that dependency and the users of those forks.
- As with most software engineering decisions, there isn’t a one-size-fits-all answer to this situation. If your project life span is short, forks are less risky. If the fork in question is provably limited in scope, that helps, as well—avoid forks for interfaces that could operate across time or project-time boundaries (data structures, serialization formats, networking protocols). Consistency has great value, but generality comes with its own costs, and you can often win by doing your own thing—if you do it carefully.

#### Revisiting Decisions, Making Mistakes

- One of the unsung benefits of committing to a data-driven culture is the combined ability and necessity of admitting to mistakes.
- For long-lived projects, it’s often critical to have the ability to change directions after an initial decision is made.

## Software Engineering Versus Programming

- Much of that difference stems from the management of code over time, the impact of time on scale, and decision making in the face of those ideas. Programming is the immediate act of producing code. Software engineering is the set of policies, practices, and tools that are necessary to make that code useful for as long as it needs to be used and allowing collaboration across a team.

## Conclusion

## TL;DRs

- Every task your organization has to do repeatedly should be scalable (linear or better) in terms of human input. Policies are a wonderful tool for making process scalable.
- Process inefficiencies and other software-development tasks tend to scale up slowly. Be careful about boiled-frog problems.

