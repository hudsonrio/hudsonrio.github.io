---
layout: post
title: What is Big Data, and what does it have to say about the Citibike gender gap?
---

This mini-post explores how, using "the cloud" (Amazon AWS clusters) and Hive (a query interface on top of Hadoop) I explored the demographics of New York's Citibike system, using this example to write a piece in plain english about what "Big Data" means in practice.

## Context

As has been noted elsewhere in [2014](http://iquantny.tumblr.com/post/82172157434/citi-bike-and-the-gender-divide), [2015](http://www.nytimes.com/2015/07/08/nyregion/a-mission-for-citi-bike-recruiting-more-female-cyclists.html) and [earlier this year](http://toddwschneider.com/posts/a-tale-of-twenty-two-million-citi-bikes-analyzing-the-nyc-bike-share-system/) **Citibike has a major gender gap**. Because this data is publicly available and fascinating, the [previous analysis](http://iquantny.tumblr.com/post/81465368612/mapping-citi-bikes-riders-not-just-rides) of this dataset is quite strong. This mini-post explores Citibike demographics  - including but not limited to gender - but uses these Citibike questions as ways to frame what "big" data tools actually are, and how they are used for the lay reader.

### Why Would you Use Big Data Tools for This? What is "Big Data" anyway?

For the purposes of this post, I define "Big Data" as any set of data that is simply too large to analyzed on a single computer. Many of the previous posts used quite large datasets and while many of the models, or functions that were applied to these datasets did _take a while to run_, "Big Data" is still distinct. Big Data, such as this Citibike dataset, simply cannot be loaded into local memory of a typical laptop such that operations can be performed (such as, for example, taking the mean duration of each trip).

![Big Data Relies On Rooms Full of Servers ](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_citi/servers.jpg?raw=true)

We are therefore left with two possible approaches to exploring gender dynamics within these ~ 30 million rides:

1. Find, rent or build a super computer
2. Use the cloud to perform distributed, parallelized processing

The first option is cost prohibitive and, unfortunately, not scalable. Let's say we want to revisit this analysis in a decade, and instead of 30 million rides, we have more like 300 million rides. Even if a super computer could keep up, it would be cost prohibitive.

The second option, however, is the almost ubiquitous method for handling "Big Data." What this approach entails is essentially that rather than create a massive computer that can do a massive operation, we must instead break up one huge task into smaller components, and distribute the work to a 'cluster' of (relatively small) Amazon servers. Among the benefits, this approach provides scalability: one can simply add servers to an existing cluster when faced with increased demand.

Hadoop is a Java-based programing framework for distributing such tasks, while Hive is essentially a layer that allows for "SQL" style queries upon the data in working memory. By using a cluster of computers, we can simply 'rent' enough server space from Amazon to fulfill practically any sized task, while overseeing the process from one's local computer. The analysis below was completed by querying in Hive.

In short, because of the millions of individual rides - in this case, each of which was represented by a row of data - aggregating the figures to answer even basic questions about the data ("What proportion of riders are women?") is requires a significant amount of processing power.

## 5 Things We Learned About Citibike (Data from June '15-June '16):

1. More than two thirds of Citibike rides (amongst subscribers) are men; the only stations with majority (or near majority) female riders are one-time events, such as a promotional screening of E.T. in Williamsburg in August 2015, or an event on Governer's Island. This is likely both because extreme values are most likely to be found in small samples.

![Women At One-Time Events ](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_citi/bike_in.png?raw=true)

2. The dock with the highest proportion of female usage was by Leonard and Boerum, on the corner of Sternberg park.

3. Riders in Brooklyn tend to be younger; the Citibike dock immediately outside the Myrtle Broadway JMZ stop, for example, has an average age among riders of about 31.

4. Riders on the Upper West Side tend to be older. All three Citibike dock on River Side Drive between 72nd and 82nd have an average rider age of over 44.


5. The area with the most disproportionately male ridership was (approximately) between 35th and 53rd, especially on the East Side, suggesting that while some combination of men who work in Midtown and male tourists ride Citibikes, few women do so, perhaps due to social pressures experienced in the workplace.

![Women At One-Time Events ](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_citi/midtown.png?raw=true)
