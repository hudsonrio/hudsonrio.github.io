---
layout: post
title: Pop Theory - (Emotionally) Neutral Song Titles are Boring
---
How you write a commercially successfully, early-00s pop single? Or, what does the data tell us about how to avoid a boring song title?


## Context

The early 2000s were pretty angsty. We had just stepped into the 21st century and, frankly, popular music was all over the place. In the heat of peak N'Sync and Destiny's Child, I wanted to create a model that could predict the hits. But how? What features I could extract that could cut through the noise?

## Hypothesis


I imagined strongly emotional music content in this period would tend to stand out. As in the examples above, strong emotional content ("need" being negative, "independent" being positive) would be associated with track-topping hits. In order to test this hypothesis, I made the assumption that track titles tended to reflect the emotional content of a song (at least over a large dataset). Armed with a dataset of popular music from the turn of the millennium, I set to test the hypothesis that **song titles associated with notably positive OR notably negative themes would be more popular than emotionally neutral titles**. In more technical terms, _my null hypothesis is that the positive or negative association of a track title is totally unrelated to chart success_.


## Some Words of Caution about Extrapolation
I wondered if this question should also help us understand the post-Y2K American psyche: do our tastes reveal something about our national character? Were our tastes as a nation optimistic or pessimistic? While these questions are interesting, one should be cautious extrapolating any conclusions I reach in this piece, given that my analysis does not include a baseline to compare how emotional content in song titles has changed over time. Indeed, my findings should be confined to the relative impact of positive and negative song titles in America around Y2K.

## Methodology


First, I looked at the relationship between how long a song was on the Billboard 100, and to what extent a song was popular (average chart position), finding they are correlated.

![Chart Showing Association Between Duration and Magnitude of Popularity (Jittered)](https://github.com/hudsonrio/hudsonrio.github.io/blob/master/images/blog%20posts/images_proj2/avgbb_weeks_jitter.jpg?raw=true "Popularity Duration and Magnitude jittered")

This lead to the construction of  a combination metric, which I named "Billboard ratio": the duration of the songs appearance on the billboard 100, divided by its average position. For any practical purposes, we might want to use a different target: for example, it might be far more profitable to be on the charts for a long time than to have a high average chart position. My metric assumes is that high positions are very valuable. While this is an imperfect metric, it serves my purpose of identifying whether my language features could predict "hits."


I used a [publicly funded dataset called Inquirer](http://www.wjh.harvard.edu/~inquirer/homecat.htm) made available by Harvard that classifies nearly every word in English by a number of categories, including positive or negative.  I compared each song title to these lists, classifying each song by how positive or negative the title was to classify, as seen below. The dataset is rich in different classifications of words, and I encourage those interested to check it out.


### Example of Positive Language

> "All the women, who are **independent**
> Throw your hands up at me
> All the honeys, who making money
> Throw your hands up at me"

**Independent** Women, Destiny’s Child 2000


### Example of Negative Language

> "I _**need**_ you like water
> Like breath, like rain

_I **Need** You_, Leann Rimes, 2000



## Analysis

Some patterns emerged: it appeared that both strongly positive or negative songs tended to outperform neutral songs, as illustrated by the following bar charts:

![More Positive Songs Tend to Be Popular, But High STDV](https://github.com/hudsonrio/hudsonrio.github.io/blob/master/images/blog%20posts/images_proj2/trackpos_bb_ratio_bar.jpg?raw=true)


![Popular Songs Stay on Charts Longer?](https://github.com/hudsonrio/hudsonrio.github.io/blob/master/images/blog%20posts/images_proj2/track_pos_weeks_jointplot.jpg?raw=true "Popularity Duration and Positivity")

Because the data is not continuous, a bar chart is a better fit for this visualization as it better illustrates by the higher mean bb_ratio amongst songs with negative language in titles. Note, however, that because the sample size of emotionally polar songs is smaller than neutral songs, the standard deviation is much higher.

![Negative Songs More Popular](https://github.com/hudsonrio/hudsonrio.github.io/blob/master/images/blog%20posts/images_proj2/trackneg_bb_ratio_bar.jpg?raw=true "Negativity and Popularity")

Based on these charts, I was unable to come to a definitive conclusion about whether increasingly polarized emotional content did indeed lead to increased (or decreased) popularity. I ran 2 paired T-Tests, comparing neutral songs to negative and positively associated chart titles respectively.

I found no statistically significant difference between positive and neutral songs (p-val of 0.55); however, **I found a noted difference between negative and neutral songs. Their mean billboard ratio scores were quite different: 1.50 for neutral, compared to 1.79 for negative songs. My second T-Test returned a p-val of 0.083** (meaning there is approximately a 8.3% chance that the observations were a result of chance).  

So given standard convention in interpreting the 'negative language' T-Test, I can not unequivocally reject the null hypothesis that emotional content is unrelated to popularity (especially because I was clearly unable to reject the null hypothesis in the 'positive language' test). Nonetheless, there does appear to be a notable pattern in the data. Further data, as noted before, would be valuable in validating this observation.

## Conclusion

I fail to reject the null hypothesis that track titles classified as 'positive' are no different from 'neutral' track titles. However, I consider the hypothesis that track titles classified as 'negative' are more popular than 'neutral' as **plausible** and recommend future research be done to explore this relationship further, both statistically and mechanistically.

One interpretation is that negative elements tend to be more poignant or relatable to listeners, and therefore more popular; however more research is required to make any definitive claim.
