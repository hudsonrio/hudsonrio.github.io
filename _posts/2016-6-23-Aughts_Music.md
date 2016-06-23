---
layout: post
title: Y2K Optimism?
---


>"All the women, who are *independent*
>Throw your hands up at me
>All the honeys, who making money
>Throw your hands up at me"
_*Independent* Women_, Destiny’s Child 2000

>"I **need** you like water
>Like breath, like rain
_I **Need** You_, Leann Rimes, 2000


The early 2000s were pretty angsty. We had just stepped into the 21st century and, frankly, popular music was both pretty bad (Smash Mouth) but also extremely memorable (“Hey Now, You’re an Allstar, Get Your Game on…”). It was the best of times, it was the worst of times.

I imagined strongly emotional content in this period would tend to stand out to the public at large. Armed with a dataset of popular music from the turn of the millennium, I set to test the hypothesis that song titles associated with notably positive OR notably negative themes would stand out on the pop charts, but that the effect size would be greater for positive songs. In more technical terms, my null hypothesis is that the positive or negative association of a track title is totally unrelated to chart success.


I wondered if this question should also help us understand the post-Y2K American psyche: do our tastes reveal something of our national character as an imagined community? Were our tastes as a nation optimistic or pessimistic? While these questions are interesting, one should be cautious extrapolating about such questions, given that my analysis does not include a baseline, our interpretations should be confined to the relative impact of positive v. Negative song titles in America in this general time frame. More data is needed to evaluate trends over time and make any sociological/ anthropological claims.


First, I looked at the relationship between how long, and to what extent a song was popular, finding they are correlated.

![Chart Showing Association Between Duration and Magnitude of Popularity](https://github.com/hudsonrio/hudsonrio.github.io/blob/master/images/images_proj2/avgbb_weeks.jpg)

This lead to the construction of  a combination metric, which I named "Billboard ratio": the duration of the songs appearance on the billboard 100, divided by its average position. While this is an imperfect metric, I used it to evaluate the popularity of songs hereafter (except where noted otherwise).

![Same Chart as Above, with a Jitter Applied to smooth the relationship](https://github.com/hudsonrio/hudsonrio.github.io/images/images_proj2/avgbb_weeks_jitter.jpg)

Using this metric, I decided to look at the most popular songs in the dataset, to see if there were any trends. Here are some of the most popular songs in the data:

Song                                                                          Artist
Maria, Maria                          Santana
Music                                 Madonna
Independent Women Part I              Destiny's Child
Bye Bye Bye                           N'Sync
Hot Boyz                              Elliott, Missy "Misdemeanor"

Next, I used a publicly funded dataset made available by Harvard that classifies nearly every word in English by a number of categories, including positive or negative.  I compared each song title to these lists, classifying each song by how positive or negative the title was to classify, as seen below.

Some patterns emerged: it appeared that both strongly positive or negative songs tended to out perform neutral songs:

![More Positive Songs Tend to Be Popular, But High STDV](https://github.com/hudsonrio/hudsonrio.github.io/blob/master/images/images_proj2/avgbb_weeks.jpgtrackpos_weeksbb_bar.jpg)


![Popular Songs Stay on Charts Longer?](https://github.com/hudsonrio/hudsonrio.github.io/images/images_proj2/track_pos_weeks_jointplot.jpg)
Because the data is not continueous, a bar chart is a better fit for this visualization.

![Negative Songs Too?](https://github.com/hudsonrio/hudsonrio.github.io/images/images_proj2/trackpos_weeksbb_bar.jpg)

Based on these charts, I was unable to come to a definitive conclusion. I ran 2 T-Tests, comparing neutral songs to negative and positively associated chart titles respectively.

I found no statistically significant difference between positive and neutral songs (p-val of 0.55); however, I found a noted difference between negative and neutral songs. Their mean billboard ratio scores were different: 1.50 for neutral, compared to 1.79 for negative songs, with a p-val of 0.083 (meaning there is 8.3% chance that the observations were a result of chance).  

So given standard convention in interpreting the negative word T-Test, I can not unequivocally reject the null hypothesis (especially as 'positive' was not predictive), but I do see a significant pattern in the data. Further data, as noted before, would be valuable in validating this observation. Final takeaway: this hypothesis is plausible, but would need to be validated with more information.

My interpretation is that negative elements tend to be more poignant to listeners, and therefore more popular, because they may be more universally relatable. More research is required to articulate a casual hypothesis for this pattern. Could just be random!
