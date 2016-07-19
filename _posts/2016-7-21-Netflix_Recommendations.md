---
layout: post
title: Which Movies Should Netflix Acquire, and Why?
---


> Identifying High-Upside Movie Acquisitions for Netflix

## Context

As this project was more open-ended than any of the previous posts, this post will focus more on the methodology - why I made the choices I did - than the details of the implementation.

From reddit:
"Metacritic tells you how good it is while RT tells you the chances of you liking."



![Graph of Relative Feature Importance: Age and Fare stand out on the Right](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj5/feature_importance.png?raw=true "Feature Importance Plot - Age and Fare Dominate")



##Conclusion

Using an ensemble method (bagging with a decision tree) and a grid search optimized logistic ridge (L2 penalty) regression model, I was able to classify survivors on the titanic with an F1 score just under 83%. The majority of my time was spent cleaning my data and implementing features.
