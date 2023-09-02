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


