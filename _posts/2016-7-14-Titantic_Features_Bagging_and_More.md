---
layout: post
title: Who Lives, Who Dies, Who Tells Your Story? (Titanic Edition)
---

#Classifying Compensation:
##Employing Various Classification Techniques to Identify Titanic Survivors, Based on Passenger Log

## Context

As is a rite of passage among those studying machine learning and data science as well as the intro kaggle competition entry,I spent this week grappling with the titanic log dataset. My mission? Identify, simply based on the passenger log, who would make it to a lifeboat and survive the Arctic disaster and who would spend their last moments on a door, Leo-style.

After investigating the dataset, I decided that without extracting some features from existing data, no model would be able to accurately classify survival. I decided to make two similar sets of features: scaled features, primarily for regression techniques, and binary 'dummy' features. I was interested how these features would preform relative to one another when run through various machine learning algorithms.

## Features I added --

While every feature imaginable has likely been extracted from this dataset, I took this as an excellent opportunity to learn a little bit more about the titanic. I set to achieve the following goals

1. Find some way to predict each person's position on the boat (what deck? left side?)
2. Improve demographic info (including finding missing ages, and fine-tuning titles)
3. Make meaningful groupings of the passengers

The first was to figure out, as best I could, where each passenger would be. Taking a look at the cabin design, it appears that decks are assigned letters, with A being on top (probably bad luck for residents of Deck G...). Second, I figured out that odd room numbers were on the starboard (right) side of the boat. By extracting these features from the ticket, I was able to orient each passenger on the boat.

Next, titles & sex...


## Imputing Missing Ages

## Models I used and their performance--

The models that performed best using cross validation on the entire dataset (using R^2 as our scoring function) were:

- grid search optimized Logistic Regression
- bagging with KNN Neighbors
- Bagging with random trees

ROC Curves

## Final predictions



## Next steps
-- Embarkation



##Conclusion

There are two clear takeaways about employment in data science more broadly: if a given job posting uses words that denote seniority or that refer to 'big-data' tools in their job postings, it is very likely they will pay more than an a similar posting without that language (at least within the domain of postings on indeed.com).

More broadly, however, having intimate control over how data is cleaned, analyzed and interpreted is incredibly important in conducting such research, as this project offered numerous opportunities to point at spurious correlations, rather than meaningful trends within the data. I would welcome feedback on my methodology in this project from any and all readers.
