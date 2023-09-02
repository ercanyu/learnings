# Chapter 1. What Is Software Engineering?

## Time and Change

- Getting through not only that first big upgrade, but getting to the point at which you can reliably stay current going forward, is the essence of long-term sustainability for your project. Sustainability requires planning and managing the impact of required change. For many projects at Google, we believe we have achieved this sort of sustainability, largely through trial and error.
- keeping software maintainable for the long-term is a constant battle

#### Hyrum’s Law

- If you are maintaining a project that is used by other engineers, the most important lesson about “it works” versus “it is maintainable” is what we’ve come to call Hyrum’s Law:
_With a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviors of your system will be depended on by somebody._
- t is conceptually akin to entropy: discussions of change and maintenance over time must be aware of Hyrum’s Law just as discussions of efficiency or thermodynamics must be mindful of entropy.
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

- 