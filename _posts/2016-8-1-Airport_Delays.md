---
layout: post
title: Make American Aviation On-Time Again
---

Much is made of America's need to make major infrastructure investments, with both progressive politicians and the unclassifiable Donald Trump agreeing that it is a major focus for this upcoming Presidential term. Hillary announced in 2015 that her platform includes a plan to allocate $55 billion over five years to provide financing for major infrastructure improvements ([Donald Trump recently offered that he wants to double this amount](http://www.nytimes.com/2016/08/03/us/politics/trump-clinton-infrastructure.html)), while Bernie has put forward an [even bigger spending bill](https://berniesanders.com/issues/creating-jobs-rebuilding-america/) he terms "The Rebuild America Act." (It is worth noting that the Presidential candidate may put forth a platform, but ultimately the power of the purse resides with Congress. Don't boo, vote.)

Hillary Clinton specifically highlights airports as key within her infrastructure proposal:

>"[My plan calls for] making smart investments in ports, airports, roads, and waterways to address the key chokepoints for the movement of goods in our economy...It means building airports and air traffic control systems that set the world standard for efficiency, reliability, and safety — saving time, money, and energy on every trip."(source)[https://www.hillaryclinton.com/briefing/factsheets/2015/11/30/clinton-infrastructure-plan-builds-tomorrows-economy-today/]

As the plan highlights, the purpose of spending on airports is to make "the movement of goods" (or people) cheaper and faster, helping America's economy become more efficient. There seems to be bipartisan consensus about the need to invest in efficiency (at least in this narrow context), but how should we best achieve these efficiency gains? Where should the first billion go?


![Using Principle Component Analysis, I Flattened My Delay Features ](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/pca_all_features.png?raw=true)


Using a dataset of national airport delays (aggregated at the airport/year level), I set out to accomplish three goals:

1) Identify the factors that contribute most to delays.
2) Offer strategic recommendations about how best to allocate limited resources.
3) Group airports that share similar operational challenges (using unsupervised learning).



## What factors contribute to delays?

My first step in understanding how delays operate was to ask: are we making progress now? I aggregated all ~800 airports in my dataset to find the average proportion of flights delayed at any airport, finding that although there seems to be a slightly positive trend, progress has been inconsistent (this was a quick, rough snapshot - for a more accurate representation, I would weight each airport by the volume of passengers traveling through it.)

![Average Proportion of Departures That Are Delayed, By Year](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/gradual_progress.png?raw=true)

I then took a look at the airports with the largest proportion of (out-going) delays, finding that Reagan, Tampa, and Cincinnati had the most frequent delays, with barely over 80% of outgoing flights going out on-time. This was a surprising result because I had never associated Reagan with (particularly) poor service. I noticed that each of these were a) relatively small airports with b) in relatively warm weather and c) on the east coast.  These observations were at odds with my expectations.

But airports do not exist in isolation: they are deeply interconnected and mutually interdependent. When a flight from Chicago is delayed, a flight in Atlanta might be held up waiting for passengers, causing the following flight to arrive late in San Juan. This phenomena of [cascading delays was described by Nate Silver](http://fivethirtyeight.com/datalab/fly-early-arrive-on-time/) when he recommended flying early in the day to mitigate the risk of delays. This phenomena is also shown in Delta's recent experience with widespread delays - each delay magnifies the problem, compounding lateness.

Indeed, when plotting the average incoming and outgoing delay frequency (where each point is the average for an airport in a given year), we can see that **outgoing delays has a similar distribution and is only slightly shifted left as compared to incoming-delays**, suggesting that most airport delays are a result of incoming delays. This is logical: an hour delay in Chicago in the morning is likely to be passed along to each of the, say, 3 other flights the aircraft is scheduled to make that day, as well as other flights which are forced to wait for passengers, or airline staff.

![Histogram of Average Delay % by Airport-Year (Arrival Delays in Blue, Departure Delays in Orange)](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/arrival_delays_drive_departure_delays.png)

Therefore, from the standpoint of evaluating operational efficiency at airports, we should only hold airports accountable for the relative difference between incoming delays and outgoing delays. For example, Westchester Airport has had fewer than 60% of flights arrive on-time 9 of the last 10 years. Even if they never contributed to a single delay, Westchester's management could not possibly achieve outgoing departure delays of much less than 60% should that trend hold true.

To this point, we've been discussing the proportion of flights that are delayed, but this is only half the story: we need to consider not only how often flights are late, but _how late_ are the late flights (on average)?

![Distribution of Magnitude of Average Delay, and Probability of a Delay](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/mag_freq_delay.png)

As show below, there is a strong correlation between the magnitude and frequency of delays, which suggests using Net Delay Score (average delay times the frequency of delay) is a better representation of true lateness (the target in our model) than using either of these factors independently. Then, after scaling the remaining features, I implemented a random forest model in order to extract feature importance. Here is the summary of the takeaways:

1. As expected, _arrival delays were an important feature, as was frequency of incoming delays__. I engineered two features to account for net lateness (departure-arrival) for the final model as a result of this observation.
2. Longitude (East-West) was very predictive of lateness (while Latitude was not). By glancing at a logistic regression implementation of this model and looking at the coefficient, I realized that eastern airports observed more lateness. However, for my final model, I did not include GPS coordinates as a feature, because I was concerned with grouping airports by operational similarities, not geographic similarities (see visualizations below for further discussion of this point).
3. The number of samples per year by airport (a proxy for the daily traffic in an airport) was also a strongly predictive factor, suggesting that larger airports were more likely to see delays.
4. Whether an airport was designated 'For Public Use' or 'Federalized/ Commercial' was not predictive of Net Delay Score, so I dropped this feature from my model.

## Airports as a 'Weak Link System' - A Digression with Sports Analogy

I am borrowing the term "weak link system" from Malcolm Gladwell's excellent _Revisionist History_ podcast ["My Little Hundred Million"](http://revisionisthistory.com/episodes/06-my-little-hundred-million) which seems itself borrowed from ["Why Everything You Know About Soccer is Wrong"](https://www.amazon.com/Numbers-Game-Everything-About-Soccer/dp/0143124560?ie=UTF8&*Version*=1&*entries*=0). I am defining a "weak link" system, for the purposes of this post as:

>"A network in which the greatest marginal efficiency improvement can be achieved by increasing efficiency in the least productive nodes, because the system is highly mutually interdependent. In contrast, **a strong link system"** is one where the greatest marginal efficiency benefits can be achieved through investment in the central or most important nodes.""

Gladwell explains the concept most intuitively by using the analogy of Soccer versus Basketball. In short, in Basketball, teams rely heavily on their best players, so the teams with the best players tend to do well (think Stephen Curry on the Warriors, or Lebron James on the Cavs); soccer, however, is a game where the quality of a team is driven more by its _worst_ player.

A very cursory look at some [soccer](https://www.whoscored.com/Regions/252/Tournaments/2/Seasons/3853/Stages/7794/TeamStatistics/England-Premier-League-2013-2014) and [basketball](http://www.basketball-reference.com/leagues/NBA_stats.html) statistics bore this example out: in soccer, most teams pass the ball roughly ten times (30-40) more per shot than in the NBA (2-4), suggesting that in order to score a goal, a lot more has to go right in soccer. Because so many players must contribute to any score in soccer - and therefore had many opportunities to be the 'weak link' that errs, allowing the other team to retake possession - ones worst player(s) have a disproportionate impact on the quality of the team as a whole.

In basketball, stars can compensate for poor or highly specialized players as evidenced by how stars like Lebron James can occupy the primary defensive and offensive roles on the court, or how others like Stephen Curry can score on possessions without a single pass. The best soccer player on earth, Lionel Messi, cannot make up for his team's defense (long Argentina's achilles heel), nor score single-handedly more than a few times a season.

### Teasing out this analogy too far

If we assume that for both sports, 80% of the time a player is passed the ball he or she makes a "good" play, maintaining possession for his team and progressing towards a shot, the difference between basketball (3 passes) and soccer (30 passes) is enormous: the basketball team will get a shot off over 50% of the time, whereas as soccer team will shoot in around 0.1% of possessions. If a better team makes a 'good' play 90% of the time, the basketball team will get a shot off over 70% of the time (a ~40% improvement), but the soccer team will shoot in over 4% of possessions, a dramatic 400% improvement. These are not fully realistic assumptions, but the point is to illustrate that in systems that are highly mutually interdependent, consistency is critically important in terms of overall efficiency.

Airports are much more like soccer than basketball. In order to improve the efficiency of the system as a whole, we should invest in the worst performing members, not in creating more 'excellent' airports. Rather than create a highly efficient, modern, high-capacity airport to replace the worst performing airport in America, we should instead spread out our investment to reap marginal efficiency improvements across as many high volume, under-performing airports as possible, because each delay is magnified elsewhere.

Let's say I have a 7:30 flight leaving from SFA. It has likely already made 3+ stops at other airports today where, at each airport, there are many opportunities to be delayed. The crew may have to wait for a pilot or plane from another (possibly delayed) flight, or wait to be cleared to use the run way, or have baggage loading issues, or a variety of other inevitable operational challenges. Something can go wrong at any step of the way, which is much more like a 30 pass soccer possession in which each player depends on their teammates to deliver the ball in such a way that the recipient even has a chance to make a positive play on the ball.

## Clustering Airports by Operational Similarities & Dimensionality Reduction

At the outset of this post, we established our goals as:

1) Identify the factors that contribute most to delays.
2) Offer strategic recommendations about how best to allocate limited resources.
3) Group airports that share similar operational challenges (using unsupervised learning).

With regard to #1, by extracting feature importance from a decision tree model (and directional effect from regression), I was able to get a sense of which features in this dataset were predictive of operational efficiency and, based on this analysis, create a list of features to cluster each airport by operational efficiency.

With regard to #2, we surmised that airports are a 'weak link' system, meaning we want to spread our resources fairly widely among struggling airports.

As noted, I engineered features for a range of unsupervised learning techniques (#3) in order to categorize airports by shared organizational challenges (or strengths), such that we can make recommendations about how to allocate resources among airports nationally. Below is a series of progressive visualizations using Principle Component Analysis, hierarchical clustering (selective features), Isomap clustering, and finally t-SNE. I included visualizations with GPS coordinates (latitude, longitude) to show how including them drastically altered the clusters.

In order to assess the proper number of clusters, I used three methods and compared the results holistically: heirarchical clustering, implementing a DB Scan, and kmeans clustering (optimizing for silhouette score, a measure which quantifies the completeness of seperation between clusters, with scores near zero reflecting overlapping groupings in n-dimensional space).

![A Truncated Dendrogram - We Have 2-4 Groups?](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/dendro.png)

After performing dimensionality reduction, I found the best silhouette score using K-means clustering (~.65) with 4 groups, while an implementation of DBScan (eps = 1, min samples=7) performed best with three distinct class labels.

One trend that became clear was that our data was dramatically transformed by include latitude/ longitude as features, as seen in the difference between the following two graphs (which visualize the data using the Isomap method).

![Isomap with GPS Features](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/isomap_gps.png)

![Isomap without GPS Features](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/isomap_nogps.png)


As we can see, the inclusion of GPS features makes these clusters dominated by the geographic location of airports, but for our purposes we want to group airports by operational similarities. By combining the t-SNE visualization method, which helps maximize the visual separation between groups in three dimensional space, a principle component analysis on my input features to minimize the effect of the "curse of dimensionality" (where the distance between points increases as the dimensionality of features increases), and using labels generated from a DB Scan clustering method, I produced the following administrative grouping recommendations.

![t-SNE - Final Grouping Recommendations](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj7/t_sne_dbscan_labels.png)

For purposes of interpretation, blue points classified as '-1' are the airports to which this analysis recommends allocating funding. As expected, all the New York Airports and other major hubs (such as ATL) were classified in the '-1' category (note that each point is an airport-year, so multiple points on this plot might represent LGA, for example). As new years are added to this model, we can reclassify each airport annually based on the performance of the prior year.

### Conclusion

These airports are highly interdependent with the rest of the system, meaning that investing in them is not simply going to improve the experience of folks traveling to and from tristate area (or Georgia), but the efficiency of the system as a whole, which is an especially important consideration from the perspective of a federal administrator. This post recommends allocating federal funds broadly amongst each of these interconnected, under-performing, high-volume airports.

Code available on github for this interested!
