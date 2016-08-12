---
layout: post
title: Make American Aviation On-Time Again - Identifying Contributors to Delays & Airport Operational Recommendations
---

Much is made of America's need to make major infrastructure investments, with both progressive politicians and the unclassifiable Donald Trump agreeing that it is a major focus for this upcoming Presidential term. Hillary announced in 2015 that her platform includes a plan to allocate $55 billion over five years to provide financing for major infrastructure improvements ([Donald Trump recently offered that he wants to double this amount](http://www.nytimes.com/2016/08/03/us/politics/trump-clinton-infrastructure.html)), while Bernie has put forward an [even bigger spending bill](https://berniesanders.com/issues/creating-jobs-rebuilding-america/) he terms "The Rebuild America Act." (It is worth noting that the Presidential candidate may put forth a platform, but ultimately the power of the purse resides with Congress. Don't boo, vote.)

Hillary Clinton specifically highlights airports as key within her infrastructure proposal:

>"[My plan calls for] making smart investments in ports, airports, roads, and waterways to address the key chokepoints for the movement of goods in our economy...It means building airports and air traffic control systems that set the world standard for efficiency, reliability, and safety — saving time, money, and energy on every trip."(source)[https://www.hillaryclinton.com/briefing/factsheets/2015/11/30/clinton-infrastructure-plan-builds-tomorrows-economy-today/]

As the plan highlights, the purpose of spending on airports is to make "the movement of goods" (or people) cheaper and faster, helping America's economy become more efficient. There seems to be bipartisan consensus about the need to invest in efficiency (at least in this narrow context), but how should we best achieve these efficiency gains? Where should the first billion go?


![Using Principle Component Analysis, I Flattened My Delay Features ](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/pca_all_features.png?raw=true)


Using a dataset of national airport delays (aggregated at the airport/year level), I set out to accomplish three goals:

1) Identify the factors that contribute most to delays,
2) Group airports that share similar operational challenges (using unsupervised learning), and
3) Offer strategic recommendations about how best to allocate limited resources.


## What factors contribute to delays?

My first step in understanding how delays operate was to ask: are we making progress now? I aggregated all ~800 airports in my dataset to find the average proportion of flights delayed at any airport, finding that although there seems to be a slightly positive trend, progress has been inconsistent (this was a quick, rough snapshot - for a more accurate representation, I would weight each airport by the volume of passengers traveling through it.)

![Average Proportion of Departures That Are Delayed, By Year](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/gradual_progress.png?raw=true)

I then took a look at the airports with the largest proportion of (out-going) delays, finding that Reagan, Tampa, and Cincinnati had the most frequent delays, with barely over 80% of outgoing flights going out on-time. This was a surprising result because I had never associated Reagan with (particularly) poor service. I noticed that each of these were a) relatively small airports with b) in relatively warm weather and c) on the east coast.  These observations were at odds with my expectations.

But airports do not exist in isolation: they are deeply interconnected and mutually interdependent. When a flight from Chicago is delayed, a flight in Atlanta might be held up waiting for passengers, causing the following flight to arrive late in San Juan. This phenomena of [cascading delays was described by Nate Silver](http://fivethirtyeight.com/datalab/fly-early-arrive-on-time/) when he recommended flying early in the day to mitigate the risk of delays. This phenomena is also shown in Delta's recent experience with widespread delays - each delay magnifies the problem, compounding lateness.

Indeed, when plotting the average incoming and outgoing delay frequency (where each point is the average for an airport in a given year), we can see that **outgoing delays has a similar distribution and is only slightly shifted left as compared to incoming-delays**, suggesting that most airport delays are a result of incoming delays. This is logical: an hour delay in Chicago in the morning is likely to be passed along to each of the, say, 3 other flights the aircraft is scheduled to make that day, as well as other flights which are forced to wait for passengers, or airline staff.

![Histogram of Average Delay % by Airport-Year (Arrival Delays in Blue, Departure Delays in Orange)](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/arrival_delays_drive_departure_delays.png)

Therefore, from the standpoint of evaluating operational efficiency at airports, we should only hold airports accountable for the relative difference between incoming delays and outgoing delays. For example, Westchester Airport has had fewer than 60% of flights arrive on-time 9 of the last 10 years. Even if they never contributed to a single delay, Westchester's management could not possibly achieve outgoing departure delays of much less than 60% should that trend hold true.

To this point, we've been discussing the proportion of flights that are delayed, but this is only half the story: we need to consider not only how often flights are late, but _how late_ are the late flights (on average)?

![Distribution of Magnitude of Average Delay, and Probability of a Delay](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/arrival_delays_drive_departure_delays.png)

As show below, there is a strong correlation between the magnitude and frequency of delays, which suggests using Net Delay Score (average delay times the frequency of delay) is a better representation of true lateness (the target in our model) than using either of these factors independently. Then, after scaling the remaining features, I implemented a random forest model in order to extract feature importance. Here is the summary of the takeaways:

1. As expected, _arrival delays were an important feature, as was frequency of incoming delays__. I engineered two features to account for net lateness (departure-arrival) for the final model as a result of this observation.
2. Longitude (East-West) was very predictive of lateness (while Latitude was not). By glancing at a logistic regression implementation of this model and looking at the coefficient, I realized that eastern airports observed more lateness. However, for my final model, I did not include GPS coordinates as a feature, because I was concerned with grouping airports by operational similarities, not geographic similarities (see visualizations below for further discussion of this point).
3. The number of samples per year by airport (a proxy for the daily traffic in an airport) was also a strongly predictive factor, suggesting that larger airports were more likely to see delays.
4. Whether an airport was designated 'For Public Use' or 'Federalized/ Commercial' was not predictive of Net Delay Score, so I dropped this feature from my model.

## Airports as a 'Weak Link System'

I am borrowing the term "weak link system" from Malcolm Gladwell's excellent _Revisionist History_ podcast ()"My Little Hundred Million")[http://revisionisthistory.com/episodes/06-my-little-hundred-million] (which seems itself borrowed from ("Why Everything You Know About Soccer is Wrong") [https://www.amazon.com/Numbers-Game-Everything-About-Soccer/dp/0143124560?ie=UTF8&*Version*=1&*entries*=0]. I am defining a "weak link" system, for the purposes of this post as:

>"A network in which the greatest marginal efficiency improvement can be achieved by increasing efficiency in the least productive nodes, because the system is highly mutually interdependent. In contrast, **a strong link system"** is one where the greatest marginal efficiency benefits can be achieved through investment in the central or most important nodes.""

Gladwell explains the concept most intuitively by using the analogy of Soccer versus Basketball. In short, in Basketball, teams rely heavily on their best players, so the teams with the best players tend to do well (think Stephen Curry on the Warriors, or Lebron James on the Cavs); soccer, however, is a game where the quality of a team is driven more by its _worst_ player.

A very cursory look at some (soccer)[https://www.whoscored.com/Regions/252/Tournaments/2/Seasons/3853/Stages/7794/TeamStatistics/England-Premier-League-2013-2014] and (basketball)[http://www.basketball-reference.com/leagues/NBA_stats.html] statistics bore this example out: in soccer, most teams pass the ball roughly ten times (30-40) more per shot than in the NBA (2-4), suggesting that in order to score a goal, a lot more has to go right in soccer. If anyWhereas,  To articulate this p took a quick look at some summary statistics for the EPL and the NBA, and found 

and the analogy that resonated most intutitively was his comparison of soccer and basketball.


## Clustering Airports by Operational Similarities

Using these observations about my features, I engineered features for a range of unsupervised learning techniques in order to categorize airports by shared organizational challenges (or strengths), such that we can make recommendations about how to allocate resources among airports nationally. Below is a series of progressive visualizations using Principle Component Analysis (all features), hierarchical clustering (selective features), Isomap clustering, and finally t-SNE. I included visualizations with GPS coordindates (latitude, longitude) to show how including them drastically altered the clusters.


images

Hierarchical Model suggests 2-4 clusters

Settled on DB Scan, 3 cateogires.


## What


 DB Scan Model- not including GPS coordinates.


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

* I recall a close friend of mine who majored in Computer Science consistently spending late nights in college. At my alma matter (Middlebury College), there were relatively few computer science majors with little interaction with other social-science driven majors. Often, CS majors were less visible socially. **This lead to my biased perception that only introverts and the technologically inclined (see 'nerds') can be successful coding** whereas I now believe that these stereotypes are simply **a reflection of self-selection bias**.

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
