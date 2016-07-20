---
layout: post
title: Which Movies Should Netflix Acquire, and Why?
---

> Identifying High-Upside Movie Acquisitions for Netflix

## Context

As this project was more open-ended than any of the previous posts, this post will focus more on the methodology - why I made the choices I did - than the details of the implementation.

We were tasked with using machine learning in order to understand what factors will predict whether a given movie is, in the words of the prompt, "good." Without delving too deeply into a Nicomachean tangent, it is important to define what we mean by good. This is where the "Subject Matter Expertise" piece of the classic data science venn-diagram comes into play:

![Data Science Venn Diagram ](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj6/data-scientist-venn-diagram.jpg?raw=true "Subject Matter Expertise")

Thanks to the folks at omdb, we can [access the OMDb API](http://www.omdbapi.com/), a free web-service with lots of movie data, through a [user-friendly Python wrapper](http://omdbpy.readthedocs.io/en/latest/) made by dgilland. This gives us convenient access to a bunch of general data about movies (actors, runtime, rating etc...) as well as a number of "ratings" from websites such as Rotten Tomatoes, Metacritic, and IMDB itself.

## Some Assumptions of Note
One factor is the cost of acquiring a given movie, but for the purposes of this analysis, we will assume that acquiring the rights to stream each movie costs about the same. Another is the fact that I was unable to find a complete list of movies that Netflix has _already_ streamed, meaning we may be offering duplicate recommendations.

Nonetheless, our goal of this study is primarily to understand **what factors help us predict a good movie** so that our model might be tuned and employed elsewhere, so these assumptions do not disqualify our analysis.

## What are we trying to predict?

Back to the "good" part. Unfortunately, because we do not have access to Netflix's proprietary data about its [80 Million Users](http://files.shareholder.com/downloads/NFLX/1378700698x0x886428/5FB5A3DF-F23A-4BB1-AC37-583BAEF2A1EE/Q116LettertoShareholders_W_TABLES_.pdf), we'll have to do the best with what we can get off the web.

However, we can intuite a basic understanding of Netflix's business. It is a subscription service so their primary business goal is essentially to acquire and retain (paying) users. We also know Netflix operates in dozens of countries, with an broad and diverse user-base. John Ciancutti referred to this problem of keeping everybody happy [in a Quora answer:](https://www.quora.com/Does-Netflix-add-content-based-on-your-searches/answer/John-Ciancutti) that:

> "Netflix seeks the most efficient content. Efficient here meaning content that will achieve the maximum happiness per dollar spent. There are various complicated metrics used, but what they are intended to measure is happiness among Netflix members.


[Zach Bulygo of kissmetrics has a good post](https://blog.kissmetrics.com/how-netflix-uses-analytics/) about in which he hypothesizes that retaining users is primarily a function of engagement: people who use Netflix will never go without it, but people who stop logging on are more likely to cancel.

>For instance, maybe [Netflix] know[s] “If we can get each user to watch at least 15 hours of content each month, they are 75% less likely to cancel. If they drop below 5 hours, there is a 95% chance they will cancel.” So now that they have this data, they can ask themselves **“How do we help users watch at least 15 hours of content per month?”**

In other words, my interpretation of Netflix's goal is not to offer one or two incredible movies to each user each month, but to maximize engagement. This means we care more about whether a movie has broad appeal than what the critics thought.

Metacritic and Rotten Tomatoes are an excellent services for aggregating critical reception of a given movie; however, critics do not always speak for the masses:

![Popular Audiences > Critics](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj6/batman_superman.jpg?raw=true)

Fortunately, we also have access to the "popcorn" score, as seen above. This metric, rather than aggregating the (weighted) average of various reviewers ("how good is the movie?"), is the proportion of respondents that had a favorable impression of the movie ("how many people think this movie is good?"). In a good concise summation from reddit:

>"Metacritic tells you how good it is while [Rotten Tomatoes] tells you the chances of you liking."

For our purposes, this is the best metric available to project what Netflix cares about. We don't truely care about whether a movie is "one of the greatest of all time," we care if it makes Netflix's users happy. However, there are limitations worth noting:

1. There may be a bias in user response, because these responses tend to be immediately after seeing the movie (where we might be most pleased or dissasified)
2. There is also likely a bias towards "Blockbusters," especially those that rely on visuals, which are best experienced in theaters and may have relatively less upside on Netflix.

Regarding the first caveat, this may actually be to our benefit: users on Netflix (probably) rarely return to content, especially as it is cycled out of rotation over time, meaning their immediate impression is the only one that matters.

Regarding the second, this "small-screen" effect does not seem to have dissuaded Netflix in the past. In 2016, they are posting Action Blockbusters ranging from Gladiator, to Braveheart, V for Vendetta, Mission Impossible III, and Django Unchained. In fact, Netflix's decision to renew Marco Polo for a second season, despite widespread critical denunciation, is because it made the people happy:


!["An all-around disappointment, Marco Polo is less entertaining than a round of the game that shares its name."](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj6/marco_polo.jpg?raw=true "Marco Polo")

Therefore, I will be constructing a Random Forest Model that attempts to predict what I will refer to as "popcorn score," or more precisesly the measure of Rotten Tomatoes users who classify a given movie as "Fresh" (divided by the number of respondents).

## Implementing the Model

(Work in progress)

## Conclusion
