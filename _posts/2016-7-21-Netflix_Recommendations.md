---
layout: post
title: Which Movies Should Netflix Acquire, and Why?
---

# Machine Learning 101: Recommending Movies on Netflix
## Identifying High-Upside Movie Acquisitions for Netflix

## Context

As this project was more open-ended than any of the previous posts, this post will focus more on the methodology - why I made the choices I did - than the details of the implementation. 

From reddit:
"Metacritic tells you how good it is while RT tells you the chances of you liking."

## Features I added --

After exploring the data, I considered what sorts of factors would _intuitively_ seem to predict survival of the titanic. I reasoned that people closer to the lifeboats would likely have a better shot of survival, as would rich folks, and finally some lucky groups (perhaps women and children?) who were either prioritized in evacuation, or simply lucky. Therefore, I set the following goals:

1. Find some way to predict each person's position on the boat (what deck? left side?)
2. Improve demographic info (including finding missing ages, and fine-tuning titles)
3. Make meaningful groupings of the passengers

The first was to figure out, as best I could, where each passenger would be. Taking a look at the cabin design, it appears that decks are assigned letters, with A being on top and G being the lowest level. Second, I figured out that odd room numbers were on the starboard (right) side of the boat (even numbers on the left). By extracting these features from the ticket, I was able to orient each passenger on the boat.

Next, I was concerned with getting a better sense of the demographics on the boat. I looked at the way names were listed on the register, and I noticed two things I could extract. First, I noticed that many women had names listed in parentheses, which I figured out were their maiden names. By checking for these names, I created a feature for whether a woman was married. Next, I noticed that imbedded in each name in the log was that person's title, which I quickly extracted.

Once I had successfully found each title, I grouped them into 7 categories. The first four were ordered by age and gender (eg. Mrs, Ms., Mr. and Master) and the final three were by profession (Reverend, Doctor, and Military). Some titles (such as "Jonkheer") did not cleanly fit into other categories, but with some research I was able to group them into these previous categories (Jonkeer is a male dutch honorific, and was therefore grouped with Mr.).

I cleaned the gender feature, and created a feature that combined the "Number of Siblings" and "Number of Parents" columns in passenger log  into a feature that simply expressed the number of family members somebody had on the boat (and likely the number of people concerned if a given person would survive). Finally, I scaled the ticket price for the scaled (regression) features, and created ticket price tiers for my binary features.

## Imputing Missing Ages

Now that I had a more developed set of features, I returned to the biggest weakness I noticed in my dataset: almost 20% of my dataset had no ages.

![Histogram Distribution of Ages with Missing Values](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj5/ages_before_clean.png?raw=true "Distribution of Ages")


At first, I considered three options:

1. simply imputing the median age (28) to all the people for whom our dataset had no ages
1. dropping each of these passengers with no ages (177 of my training set of 891) from the dataset and keeping Age as a feature
1. dropping age as a feature

My first option was inadequate because it meaningfully changed the distribution of age across the boat. Look at the histogram above - about 3.5% of people on the boat were 28; however, by imputing ages, this number would rise to over a fifth of the boat. This would have meaningfully changed how age behaved as a featue, likely hurting out model's predictions.

The second and third options were not reasonable for similar reasons: I had a limited number of features, and felt that dropping age or roughly 20% of my dataset would lead to a loss of accuracy that I could not make up no matter how sophisticated and well-tuned my model.

Instead, I settled on leaning on the work I had already done - extracting title - as the best way to impute more realistic age values. By computing the average age among those with Military titles, for example, I imputed an average age of 56.6. This allowed me to differentiate within gender: those with 'Ms' as their title had an average age of about 22, whereas the 'Mrs.' group was about 36.

## Models I used and their performance--

Below is a confusion matrix, using logistic regression with no penalty. This gives me a sense of a baseline: if a model cannot beat this performance, it is unlikely to be valuable for my purposes.

![Confusion Matrix (Test Data) Using Logistic Regression with no Penalty](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj5/confusion_matrix_log.png?raw=true "Basic Confusion Matrix")


My strategy was to train each model on the full training set, and evaluate their success using cross validation (using a stratified, shuffled Kfold method) on the entire dataset (using F1 Score as our scoring function). This allowed me to hone in on two techniques which I found stood out in their accuracy.

- Grid search-optimized logistic regression
- Bagging (with a basic decision tree model)

Near the end of this piece, I will return to explore why these models outperformed the others.


## Dummies versus Scaled features

As mentioned, I thought this project would be an excellent opportunity to compare the efficacy of binarized data  versus using continuous values, and constructed two sets of features to compare these two approaches in the context of a many-featured model.

### A quick aside: if you have no idea about what I mean about binarized data...

If you are comfortable with the distinction between binarized and continuous features, feel free to skip ahead to the conclusion. This section will explain the difference in constructing the two, so the interpretation of what I found makes more sense.

Let's take the 'fare' variable, or the amount the person paid for their ticket. Logically, we would assume that the more somebody paid for their ticket, the higher their 'class'; we would expect people within a given 'class' to either survive together or die together, perhaps with the higher socio-economic classes having better survival outcomes that people riding coach.

We could prepare the data then in two equally valid ways:

1. Group passenger fares into buckets, and create dummy variables for each bucket
1. Input the fare paid into the model as a continuous variable (and scale it).

Let's take a step back and think about this in the abstract with a modern, mostly unrelated example:

In a modern travel context, the first approach would likely make much more sense. In an airplane, there are 3 classes of seating: first class, business class, and economy. These are clean divisions that would make a lot of sense: we expect people in each of these classes to share something in common with their fellow class mates.

In contrast, looking at the exact amount they paid for their ticket might not make sense: a person might have booked yesterday and be stuck paying a ton for coach, whereas a first-class passenger might have booked months ago, and have received a strong deal.

In this modern case it might make more sense to group these things with categorical data (which class did you buy a ticket for?). So then why don't we say first class = 1 and economy = 3? and just throw this feature into the model? Sklearn would have no problem processing this data; the problem is it sends the wrong message.

If class were a set of **ordinal** values, this would be appropriate: if economy were truely 3 times more (of something) than first class, this would be an appropriate way to slice our data. However, economy is not 3x more _anything_ than first class.

Instead, we want data that simply says "which class are they in?" with three possible values. The way we represent this concept in sklearn is using binarized data, or a sparse matrix. Instead of having three values in a column asking "what class?", we would have three columns asking "Are they in first class?" (no), "Are they in business class?" (yes), and "Are they in economy" (no). For yeses, we write 1s. For No's, we write zeros.

There you have it: binarized data (or dummy variables) are just a ton of columns that ask "Is this row (person) a part of the columns category (class)". In my binarized set of features, there were 30 such columns, such as "Woman" (1 = yes, 0 = man) or "Starboard Cabin" (1 = their cabin is on right side, 0 = left side of the boat).

Returning to the question at hand: how should I deal with my 'Fare' variable, or more precisely: is it best considered an **ordinal** or **contiuous** variable. My intution is that it is best captured as a continuous variable for two reasons:

1. By 'bucketing' I am introducing arbitrary splits into the model ("should cheap tickets be $5 and less, or $7 and less?").
1. People were shared a relatively continuous set of values, so therefore class seems to be more complicated than a few simple groupings.

Nonetheless, I prepared fare (among my other features) in both ways, so I could test this intuition based on model efficacy.


## How did continuous and categorically cleaned X values effect the accuracy of various models?

![Table of Model Predictive Accuracy (Cross-validated F1 Scores)](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj5/model_output_ranked.png?raw=true "Cross-validated F1 Scores")

The table above is the ranked output of each of these models, ranked by their F1 score (which is a composite score of precision and accuracy of both prediction classes).

As you can see above, the mean F1 score was higher for the models using scaled features than dummy features, and the top 3 performing models used scaled features; however, different models had different outcomes based on the scaled vs. dummy input. For example, a simple decision tree model with categorical inputs outperformed the same model with continuous data.

The primary takeaway is that neither scaled or binarized data is inherently better than the other; it depends on the feature in question. Scaled data, however, was the way that I was able to create my most accurate model (as judged by F1 Score).


## Why did these model outperform the others?

Two models stood out: Bagging (which is an ensemble method that uses decision_trees), and Logistic Regression (optimized by grid search).

Here are visualizations of the ROC curves of the Bagging model. The area under the curve is a good way for assessing the predictiveness of the model:

![ROC Curve for Bagging Trees Model](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj5/bagging_tree_roc.png?raw=true "ROC Curve for Bagging Trees Model")


The big challenge in this was in the relative weight of the 30 features that I extracted from the dataset. It appears that a critical element in this dataset was using bagging, or training our model (in this case using both KNN and a basic decision tree algorithm) on a number of randomly selected samples (with replacement) of the input features, and aggregating the results of these models helped ensure appropriate weighting of each feature.

We see a similar phenomena in the optimized logistic regression, where we were using a ridge penalty, which is best suited for problems where the relative weighting of a number of features is important.

The decision tree model yielded an output of the relative value of each feature, and I noticed that the relative importance of features was bimodal: two features (scaled age and scaled fare) accounted for roughly 50% of the assigned importance (age and fare paid), while the other 28 features accounted for the other ~50% of importance in the model

![Graph of Relative Feature Importance: Age and Fare stand out on the Right](https://raw.githubusercontent.com/hudsonrio/hudsonrio.github.io/master/images/blog%20posts/images_proj5/feature_importance.png?raw=true "Feature Importance Plot - Age and Fare Dominate")

This explains why there was a lot of consensus in my models in reaching classification accuracy levels around 75%, but why getting to 82% was very challenging: most of the models were driven primarily by age and scale, but it was the ability to best employ the other features that I was able to marginally improve the model.


## Next steps
One feature I did not have time to clean and include in this model is Port of Embarkation. While I imagine it will be correlated with the fare paid, it may add some additional improvement to the model (although I suspect it will be marginal).

Next, I would use recursive feature selection to identify which features I need to include in my model, or could live without. Currently, my model may contain features that are unnecessary.

This analysis did not include an implementation of a Support Vector Machine model, but this is undoubtedly a technique I would add in future research.

Finally, I would spend time 'pruning' my decision tree models, passing optional parameters into my bagging tree model using grid search. For all these revision, please check out my final model submission to Kaggle!


##Conclusion

Using an ensemble method (bagging with a decision tree) and a grid search optimized logistic ridge (L2 penalty) regression model, I was able to classify survivors on the titanic with an F1 score just under 83%. The majority of my time was spent cleaning my data and implementing features.
