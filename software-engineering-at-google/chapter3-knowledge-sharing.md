# Chapter 3. Knowledge Sharing

## Challenges to Learning

#### Lack of psychological safety

- An environment in which people are afraid to take risks or make mistakes in front of others because they fear being punished for it. This often manifests as a culture of fear or a tendency to avoid transparency.

#### Information islands

###### Information fragmentation

- Each island has an incomplete picture of the bigger whole.

###### Information duplication

- Each island has reinvented its own way of doing something.

###### Information skew

- Each island has its own ways of doing the same thing, and these might or might not conflict.

#### Single point of failure (SPOF)

- A bottleneck that occurs when critical information is available from only a single person.
- it can be easy to fall into a habit of “Let me take care of that for you.” But this approach optimizes for short-term efficiency (“It’s faster for me to do it”) at the cost of poor long-term scalability (the team never learns how to do whatever it is that needs to be done).

#### All-or-nothing expertise

- This problem often reinforces itself if experts always do everything themselves and don’t take the time to develop new experts through mentoring or documentation.

#### Parroting

- This is typically characterized by mindlessly copying patterns or code without understanding their purpose

#### Haunted graveyards

- Places, often in code, that people avoid touching or changing because they are afraid that something might go wrong. Unlike the aforementioned parroting, haunted graveyards are characterized by people avoiding action because of fear and superstition.

## Philosophy

- Documented knowledge, on the other hand, can better scale not just to the team but to the entire organization.
- But even though written documentation is more scalable than one-to-one conversations, that scalability comes with some trade-offs: it might be more generalized and less applicable to individual learners’ situations, and it comes with the added maintenance cost required to keep information relevant and up to date over time.

## Setting the Stage: Psychological Safety

- An enormous part of learning is being able to try things and feeling safe to fail. In a healthy environment, people feel comfortable asking questions, being wrong, and learning new things.

#### Mentorship

- One important way of building psychological safety is to assign Nooglers(new Googler) a mentor—someone who is not their team member, manager, or tech lead—whose responsibilities explicitly include answering questions and helping the Noogler ramp up.

#### Psychological Safety in Large Groups

- The most important way to achieve this safe and welcoming environment is for group interactions to be cooperative, not adversarial.
- Antipatterns can emerge unintentionally: someone might be trying to be helpful but is accidentally condescending and unwelcoming. We find the Recurse Center’s social rules to be helpful here:
  - No feigned surprise (“What?! I can’t believe you don’t know what the stack is!”)
  - No “well-actuallys”
  - No back-seat driving(Interrupting an existing discussion to offer opinions without committing to the conversation.)
  - No subtle “-isms” (“It’s so easy my grandmother could do it!”)

## Growing Your Knowledge

#### Ask Questions

- If you take away only a single thing from this chapter, it is this: always be learning; always be asking questions.

#### Understand Context

- Learning is not just about understanding new things; it also includes developing an understanding of the decisions behind the design and implementation of existing things.
- Consider the principle of “Chesterson’s fence”: before removing or changing something, first understand why it’s there.
- Engineers have a tendency to reach for “this is bad!” far more quickly than is often warranted, especially for unfamiliar code, languages, or paradigms.
- Seek out and understand context, especially for decisions that seem unusual. After you’ve understood the context and purpose of the code, consider whether your change still makes sense. If it does, go ahead and make it; if it doesn’t, document your reasoning for future readers.

## Scaling Your Questions: Ask the Community

- when you learn something from a one-to-one discussion, write it down.

#### Group Chats

#### Mailing Lists

- Most topics at Google have a topic-users@ or topic-discuss@ Google Groups mailing list that anyone at the company can join or email. Asking a question on a public mailing list is a lot like asking a group chat: the question reaches a lot of people who could potentially answer it and anyone following the list can learn from the answer.

#### YAQS: Question-and-Answer Platform

- YAQS (“Yet Another Question System”) is a Google-internal version of a Stack Overflow–like website, making it easy for Googlers to link to existing or work-in-progress code as well as discuss confidential information.

## Scaling Your Knowledge: You Always Have Something to Teach

#### Office Hours

- Office hours are a regularly scheduled (typically weekly) event during which one or more people make themselves available to answer questions about a particular topic.

#### Tech Talks and Classes

#### Documentation

#### Code

## Scaling Your Organization’s Knowledge

#### Cultivating a Knowledge-Sharing Culture

#### Establishing Canonical Sources of Information

- Canonical sources of information are centralized, company-wide corpuses of information that provide a way to standardize and propagate expert knowledge.

#### Staying in the Loop

## Readability: Standardized Mentorship Through Code Review

- At Google, “readability” refers to more than just code readability; it is a standardized, Google-wide mentorship process for disseminating programming language best practices.
- Readability covers a wide breadth of expertise, including but not limited to language idioms, code structure, API design, appropriate use of common libraries, documentation, and test coverage.
- Readability started as a one-person effort. In Google’s early days, Craig Silverstein (employee ID #3) would sit down in person with every new hire and do a line-by-line “readability review” of their first major code commit. It was a nitpicky review that covered everything from ways the code could be improved to whitespace conventions. This gave Google’s codebase a uniform appearance but, more important, it taught best practices, highlighted what shared infrastructure was available, and showed new hires what it’s like to write code at Google.
- Today, around 20% of Google engineers are participating in the readability process at any given time, as either reviewers or code authors.

#### What Is the Readability Process?

- Code review is mandatory at Google. Every changelist (CL) requires readability approval, which indicates that someone who has readability certification for that language has approved the CL.
- This requirement was added after Google grew to a point where it was no longer possible to enforce that every engineer received code reviews that taught best practices to the desired rigor.
- Within Google, having readability certification is commonly referred to as “having readability” for a language. Engineers with readability have demonstrated that they consistently write clear, idiomatic, and maintainable code that exemplifies Google’s best practices and coding style for a given language.
- Around 1 to 2% of Google engineers are readability reviewers. All reviewers are volunteers, and anyone with readability is welcome to self-nominate to become a readability reviewer.

#### Why Have This Process?

- Code is read far more than it is written, and this effect is magnified at Google’s scale and in our (very large) monorepo
- The value of codebase-wide consistency cannot be overstated: even with tens of thousands of engineers writing code over decades, it ensures that code in a given language will look similar across the corpus.

## Conclusion

-  A culture that promotes open and honest knowledge sharing distributes that knowledge efficiently across the organization and allows that organization to scale over time.

## TL;DRs

- Psychological safety is the foundation for fostering a knowledge-sharing environment.
- Start small: ask questions and write things down.
- There is no silver bullet: empowering a knowledge-sharing culture requires a combination of multiple strategies, and the exact mix that works best for your organization will likely change over time.