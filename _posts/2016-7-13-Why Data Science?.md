---
layout: post
title: Why would a Political Science graduate start coding?
---

It can be really hard to make "the leap" from a non-technical role to Data Science. This post highlights how I've revised some of my preconceptions, how I stumbled upon the "power" of data, and why I believe a Liberal Arts background is invaluable in data.

Around a year ago, after working my way through the ranks in sales at a startup called [Mighty](https://mighty.com/), I got the chance to work on our operational processes to rehaul, refine and document our standard operating procedure. My primary asset, I realized, in figuring out how to best achieve these goals was **data**.

I hacked my way using google spreadsheets, periscope.io (and some basic ability to query our postgreSQL database), to contribute to our 'innovation pipeline': a list of prioritized hypotheses, a process for vetting methodology, and a systematic approach to documenting learnings.

However, our analytical capacity - we had a team of passionate, young, curious, largely inexperienced folks - far exceeded the data we had at hand, and the rate we could gather data through controlled experimentation. The reason was not that we had not thought to collect enough data, but that our data pipeline was accessible only to more technical members of the team.

We also ran into issues of data integrity; for example, client-entered data (e.g. "Who is your attorney?") lead to dozens of duplicates or spelling variations. Data we could not trust fell into disuse, and our leading to a self-reinforcing cycle of "pushing off" our technical debt by not addressing these issues at the outset. For example, some of our sales KPIs depended on consistent data entry in 2 disconnected CRMs. Because we could not trust these KPIs because of lack of data entry in our secondary CRM (Salesforce), a vicious cycle emerged: Salesforce was not updated consistently, meaning we were hesitant to trust our data.

!["I'll Send You Those Growth Projections This Week"](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/crm.png?raw=true)


Three things became clear:

1. We were spending too much amount of time and resources collecting data manually.

2. We could not trust (all of) our data definitively, constraining those in management positions to make data-driven decisions in certain aspects of the company.

3. A single additional employee with foresight, skepticism, and a 'Data Science' skill-set could make a dramatic difference.

Thanks to input from some of my teammates (thanks to Darlin and Ish especially for pushing me in the right direction), I began to question an assumption I had long held:


**I am no coder. I don't have the background, I don't think like a coder, and it will be soul-crushingly boring.**

![Command Line, First Day ](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/coders_only.jpg?raw=true)

## Unpacking my Preconceptions about Computer Science:



#### 1) I don't have the background

* I did not take a computer science class in college, was deeply unfamiliar with the industry jargon, and did not have a good mental model of how different languages work together, or what exactly a given coder was responsible for.

* I realized, however, that I had some skills that few coders had that could set me apart: I was familiar (at a high-level) with frequentist and bayesian statistical approaches, I had project management experience, and knew how these skills fit into a start-up.

* While I avoided computer science in high school and college, I realized that it was not a lack of intellect or aptitude that kept me away from coding, but because of how I defined myself: I had a self-conception as a qualitative writer who leaned on quantitative elements to prove a point, but I did not permit myself to explore the power of programming as an intellectual path. I realized I had dismissed "computer science" or "coding" just because many elements I associated with computer science (such as front-end development or mobile app development) did not fit my interests.

####  2) I don't think like a coder

* I assumed, as I believe many do, that computer programmers are in some ways an order of magnitude more intelligent than most people. I imagined that in the same way that one might never be competitive with Tiger Woods in golf without picking the game up at age 5, **if one did not pick up coding early, catching up to even middle-tier coders would be nearly impossible**.

![Full Stack Dev](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/tiger.png?raw=true)


* I have since revised this assumption: while there are many full-stack developers/machine learning savants/data engineering wizards out there, I now see the difference between myself and they as  **fundamentally a function of experience, rather than of 'talent' or 'intelligence'**. There may be some intellectual cut off point below which it is challenging to succeed in computer science, but principally, most develop their skills because they have put in the time to learn. Teams can compensate for shortcomings of individual teammates, while allowing them to focus primarily on their strengths.

* More personally, I found myself fascinated by the power of weaving stories out of data. I have long enjoyed writing, and as I became more comfortable working with data, I began to see the process of data preparation & analysis as similar to the process of writing in that both require weaving intellectual threads together through a process that is as much an art as a science.

* I also realized that my basic conception of the workflow of coding was misguided: **I now believe coding is more like writing a paper in a foreign language than making an oral argument in that language**. For example, making a sophisticated oral argument in Spanish is truly daunting for non-fluent speakers(like myself): the amount of information one has to retain in "biological CPU" is overwhelming: Spanish syntax, vocabulary & conjugations, topic-specific terminology, and the overarching logic of one's argument. In contrast, writing a paper in Spanish is far easier for natural english speakers, because it can be more easily separated into sub-tasks. One can sketch out the broad argument (in english), and slowly translate, sentence-by-sentence, until you have a full paper. **Learning how to break tasks down into smaller components is a critical skill and can make writing even complex code less daunting.** In fact, there's no shame in occasionally referring to documentation, or referring to the code of others.**

* I realized that the quickly-moving field of Data Science offers an opportunity to new entrants because of the ability to leap-frog older techniques. Open-source languages are evolving constantly, and new software is constantly being implemented, so even though truly new techniques might be rare, new implementations or tweaks are constant. Therefore, **an individual's utility is disproportionately influenced by their work in the last 18-24 months**. This advantages recent entrants to the field, which helps compensate for lack of a decades of experience.

* Finally, I realized that there is perhaps no better background for Data Science than a Liberal Arts skill-set. I define a liberal arts skill-set as: creativity, analytical rigor, capacity to research independently and learn quickly, and the ability to understand or articulate nuanced points. **What separates a valuable data scientist is her or his ability to weave a story together out of unclean, often opaque data that means something to her audience**. The challenge inherent in machine learning, for example, is just not getting the best "score", but how to extract actionable insights, and avoid interpreting noise as signal, which often relies on ones **analytical reasoning or knowledge of a given domain**, aided by data skills.

#### 3) It will be soul-crushingly boring

* I had the impression that 'Computer Science' was an extremely broad track which taught students to have R2D2-style neural bonds with computers, allowing these enlightened individuals to do everything from making websites pretty, to making amazon recommendations, to search algorithms and dynamic interfaces. **I now understand that 'coding' is a highly specialized group of tasks, functions and roles which allow for a similar diversity of specialization as the social science field as a whole** (for example). Even though neither Flash, nor front-end development are particularly appealing to me, I did not have a good mental image of the extent to which one can specialize.

![Computer Science is Easy!](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/R2.png?raw=true)

* I recall a close friend of mine who majored in Computer Science consistently spending late nights in college. At my alma matter (Middlebury College), there were relatively few computer science majors with little interaction with other social-science driven majors. Often, CS majors were less visible socially. **This lead to my biased perception that only introverts and the technologically inclined (see 'nerds') can be successful coding** whereas I now believe that these stereotypes are simply **a reflection of self-selection bias**.

![Cool Kids Code](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/cool_kids.png?raw=true)

* Last, **I undervalued the sense of achievement when one completes a project**. It feels really good in its own right to make something "work". But the feeling of sharing a project with somebody else where it is helpful (to them or to your team as a whole) combines the satisfaction of contributing to a team, and the rush of discovery. As a basketball player, the most universal analogy I can draw is that producing a valuable model is like being passed the ball and making a game-winning shot. You feel both triumphant individually, and included in something bigger: the success of your team. It feels really good.



![The GOAT](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/jordan.jpg?raw=true)



## What should I keep in mind if I only skimmed the part above? Its 2016 and long-form blog posts are dead. Listicle plz?

1. Computer Science is a broad field, full of sub-specializations. There is likely somewhere where one's personal preferences can fit (whether Data Science or elsewhere).
2. There is a significant selection-bias in those who "see themselves" doing computer science. Do not ascribe to such stereotypes.
3. There are always going to be coders who are way better than you, but they are better because they have put in more time, not because they are just plain smarter (for the most part). You do not have to be the best to be valuable.
4. You don't have to keep _all_ of it in your head: referring to documentation or past code makes things easier, especially for new domains.
5. Working with others can help compensate for one's weaknesses, and to support collaborators strengths. Teams are often positive-sum affairs in terms of efficiency. You can also get a lot by reading the code of others (e.g. Cross Validated, Stack Overflow, Kaggle, Documentation examples).
6. Because new methods, packages, and implementations are constantly coming out, there is disproportionate value in "recent" work, making it easier for new entrants to "catch-up."
7. Coding can be highly rewarding at an emotional level (at least that is my experience).
8. You can add a lot of value to a real-world team, by combining not only computer science skills, but through more generalized data-driven and systematic thinking. Foresight in data collection, for example, can be more valuable than any model.

A central part of this blog post was in articulating my own mental model of the data science field and coding more broadly. If you (who likely know much more than I) believe I could add more nuance to this model, or that something I argued is just plain wrong, please reach out to me (details on my about page).
