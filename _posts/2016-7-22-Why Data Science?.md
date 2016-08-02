---
layout: post
title: Why would a Political Science graduate start coding?
---
> My road to Data Science

Around a year ago, after working my way through the ranks in sales at a startup called [Mighty](https://mighty.com/), I got the chance to work on our operational processes to rehaul, refine and document our standard operating procedure. My primary asset, I realized, in figuring out how to best achieve these goals was **data**.

I hacked my way using google spreadsheets, periscope.io (and some basic ability to query our postgreSQL database), to contribute to our 'innovation pipeline': a list of prioritized hypotheses, a process for vetting methodology, and a systematic approach to documenting learnings.

However, our analytical capacity - we had a team of passionate, young, curious, inexperienced folks - far exceeded the data we had at hand, and the rate we could gather data through controlled experimentation. The reason was not that we had not thought to collect enough data, but that we had not fully automated nor considered the tradeoffs of our data pipeline. In some places, client-entered data lead to dozens of duplicates or spelling variations. Data we could not trust was fell into disuse, leading to a self-reinforcing cycle of "pushing off" our technical debt by not addressing these issues at the outset. For example, our sales KPIs depended on consistent data entry in 2 disconnected CRMs (which never happened consistently). Because we could not trust these KPIs because of lack of data entry, a vicious cycle emerged: nobody updated Salesforce consistently, because they had next to no incentive to do so, meaning our data was largely useless.

!["I'll Send You Those Growth Projections This Week"](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/crm.png?raw=true)


Three things became abundantly clear:

1. We were spending an enormous amount of time and resources collecting (bad) data.

2. We could not trust (all of) our data definitively, forcing those in management positions to rely too heavily on intuition-based strategy in too many areas across the company.

3. A single employee with foresight, skepticism, and a 'Data Science' skill-set could make a dramatic difference.

Thanks to input from some of my teammates (thanks to Darlin and Ish especially for pushing me in the right direction), I began to question an assumption I had long held:


**I am no coder. I don't have the background, I don't think like a coder, and it will be soul-crushingly boring.**

![Command Line, First Day ](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/coders_only.jpg?raw=true)

## Unpacking my Preconceptions about Computer Science:



#### 1) I don't have the background

* I did not take a computer science class in college, was deeply unfamiliar with the industry jargon, and did not have a good mental model of how different languages work together, or what exactly a given coder was responsible for.

* I realized, however, that I had some skills that few coders had that could set me apart: I was familiar (at a high-level) with frequentist and bayesian statistical approaches, I had project management experience, and knew how these skills fit into a start-up.

* While I avoided computer science in high school and college, I realized that it was not a lack of intellect or aptitude that kept me away from coding, but because of how I defined myself: I had a self-conception as a qualitative writer who leaned on quantitative elements to prove a point, but I did not permit myself to explore the power of programming as an intellectual path. I realized I had dismissed "computer science" or "coding" just because many elements I associated with computer science (such as front-end development or mobile app development) did not fit my interests.

####  2) I don't think like a coder

* I assumed, as I believe many do, that computer programmers are in some ways an order of magnitude more intelligent than most. I imagined that in the same way that one might never be competitive with Tiger Woods in golf without picking the game up at age 5, **if one did not pick up coding early, catching up to even middle-tier coders would be nearly impossible**.

![Full Stack Dev](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/tiger.png?raw=true)


* I have since revised this assumption: while there are many full-stack developers/machine learning savants/data engineering wizards out there, I now see the difference between myself and they as  **fundamentally a function of experience, rather than of 'talent' or 'intelligence'**. There may be some intellectual cut off point below which it is challenging to succeed in computer science, but principally, some people are excellent at these skills because they put in their "10,000 hours." I now believe that one does not have to reach 10,000 hours to be employable in Data Science, because a) **teams can compensate for shortcomings of individual teammates** and b) because **somebody has already approached most problems out there, and probably did a decent job, which one can learn from** and even emulate.

* More personally, I found myself truly enthralled by the power of the ability to weave stories out of data. I have long been able to write papers for extended periods of time, weaving intellectual threads together

* I also realized that my basic conception of the workflow of coding was misguided: **I now believe coding is more like writing a paper than making an oral argument**. For example, making a sophisticated oral argument in Spanish is truly daunting for non-fluent speakers(like myself): the amount of information one has to retain in "biological CPU" is incredible: Spanish syntax, vocabulary & conjugations, topic-specific terminology, and the overarching logic of one's argument. In contrast, writing a paper in Spanish is far easier for natural english speakers, because it can be more easily separated into sub-tasks. One can sketch out the broad argument (in english), and slowly translate, sentence-by-sentence, until you have a full paper. In so doing, you are learning. When you forget a translation, you can thumb over to a pocket dictionary. And once you're done, you can modify sentence structure from simple, repetitive sentence structure to a more efficient, articulate way of phrasing your point. All this is to say **referring to documentation quickly and easily while coding makes it seem far less daunting, especially for beginners.**

* Finally, I realized that the quickly-moving field of Data Science offers an opportunity to new entrants because of the ability to leap-frog older techniques. For example, Pandas (an implementation in Python that makes manipulating, cleaning and graphing easier) did not exist prior to 2008, meaning the whole world had to learn it in 2009. Open-source languages are evolving constantly, and new software is constantly being implemented, so even though truly new techniques might be rare, new implementations or tweaks are constant. Therefore, **an individual's utility is disproportionately influenced by their work in the last 18-24 months**. This advantages recent entrants to the field, which helps compensate for lack of a decades of experience.

#### 3) It will be soul-crushingly boring

* I had the impression that 'Computer Science' was an extremely broad track which taught students to have R2D2-style neural bonds with computers, allowing these enlightened individuals to do everything from making websites pretty, to making amazon recommendations, to search algorithms and dynamic interfaces. **I now understand that 'coding' is a highly specialized group of tasks, functions and roles which allow for a similar diversity of specialization as the social science field as a whole** (for example). Even though neither Flash, nor front-end development are particularly appealing to me, I did not have a good mental image of the extent to which one can specialize.

![Computer Science is Easy!](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/R2.png?raw=true)

* I recall a close friend of mine who majored in Computer Science spending late nights just about every night in college. At my alma matter (Middlebury College), there were few computer science majors with little interaction with other social-science driven majors. Often, CS majors were less visible socially. **This lead to my biased perception that only introverts and the technologically inclined (see 'nerds') can be successful coding** whereas I now believe that these stereotypes are simply **a reflection of self-selection bias**.

![Cool Kids Code](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/cool_kids.png?raw=true)

* Last, **I undervalued the sense of achievement when one completes a project**. It feels really good in its own right to make something "work". But the feeling of sharing a project with somebody else where it is helpful (to them or to your team as a whole) combines the satisfaction of contributing to a team, and the rush of discovery. As a basketball player, the most universal analogy I can draw is that producing a valuable model is like being passed the ball and making a game-winning shot. You feel both triumphant individually, and included in something bigger: the success of your team. It feels really good.



![The GOAT](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_why_ds/jordan.jpg?raw=true)



## What should I keep in mind if I only skimmed the part above? Its 2016 and long-form blog posts are dead. Listicle plz?

1. Computer Science is a broad field, full of sub-specializations. There is likely somewhere where your personal preferences can fit (whether Data Science or elsewhere).
2. There is a significant selection-bias in those who "see themselves" doing computer science. Do not ascribe to such stereotypes.
3. There are always going to be coders who are way better than you, but they are better because they have put in more time (not bceause they are just plain smarter, for the most part). You do not have to be the best (yet) to be valuable.
4. You don't have to keep _all_ of it in your head: referring to documentation or past code makes things easier as you gradually become comfortable with a broader set of methods, packages or functions.
5. Work with others to compensate for your weaknesses, and to support their strengths. Teams are often positive-sum affairs in terms of efficiency. You can also get a lot by reading the code of others (e.g. Cross Validated, Stack Overflow, Kaggle, Documentation examples).
6. Because new methods, packages, and implementations are constantly coming out, there is disproportionate value in "recent" work, making it easier for new entrants to "catch-up."
7. Coding can be highly rewarding at an emotional level (for many).
8. You can add a lot of value to a real-world team, by combining not only computer science skills, but through more generalized data-driven and systematic thinking. Foresight in data collection, for example, can be more valuable than any model. In doing so, you can enable team-members to overachieve.

A central part of this blog post was in articulating my own mental model of the data science field and coding more broadly. If you (who likely know much more than I) believe I could add more nuance to this model, or that something I argued is just plain wrong, please reach out to me (details on my about page).
