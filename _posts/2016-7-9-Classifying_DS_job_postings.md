---
layout: post
title: Can we identify high-paying Data Science roles by scraping public job descriptions?
---

#Classifying Compensation:
##Employing Logistic Regression to Identify 'Above-Average' Paying Data Science Roles posted on Indeed.com

##Context

Negotiating for salary can be very challenging; a little bit of knowledge about where the person across the table is coming from can go a long way. But an important element of reaching consensus is making sure that both parties are close enough at the outset that compromise is plausible. For example, for a job applicant to have an idea of the value of a given role can help he or she find an opportunity that matches or exceeds their expectations.

Using scraped data from Indeed.com, I set to test my own hypotheses about what types of skills are most valued within the Data Science field. Is 'Big Data' necessary for an above-average compensation package, or do companies value the ~80% of a time data scientists spend cleaning, mining and preparing data for analysis?

##Hypothesis

I believe strongly in articulating our assumptions or understandings about a given problem or dataset at the outset, because even though one could refine their hypotheses **after** interpreting the results, there is little to be learned in such image-regulation. Instead, I will use this piece to go through my (sometimes misguided) thought-process.

Although there are important drivers of compensation, such as the type of company, the city (including its cost of living, and competition from other potential employers), and the applicant's experience, I have choosen to confine my analysis to interpreting **job descriptions** and **titles**. Adding additional features would undoubtedly strengthen this model, but my priority is testing my _a priori_ assumptions about the field.

I proposed the following:
1. **Big Data** Roles with 'Big Data' keywords ('big', 'hadoop', 'spark' etc...) are associated with greater salaries than Data Science roles as a whole.
1. **Data Skills** Roles that enumerate specific data skills ('python', 'r', 'cleaning', 'scikit-learn' etc...) are also associated with above-average salaries
1. **Seniority** Roles that include terms the denote seniority ('senior', 'lead', 'principle', 'VP' etc...) would also lead to above-average salaries.

Based on the parameters of the project from General Assembly, I set out not to estimate salaries for each given role, but to classify them as either above or below average, using Logistic Regression (using scikit-learn).

Going through my dataset, I found that roughly a third of scraped job postings fell into each of these categories. I was able to access roughly 4,500 individual job postings. Because the distribution of annual salary is right skewed the calculated average salary classifies 63% of job postings as 'above-average' (our baseline accuracy), meaning if my model cannot beat guessing 'above-average' 63% of the time, it is a pretty useless model.

![Disitribution of Salary within my Scraped Dataset](******** in folder****** "Distrubution of Listed Salary (Indeed.com)")

###Side Note: Binary versus Ordinal Values

I faced a methodological choice in my string parsing: if two job descriptions indeed matched a value within my "seniority" list, but one had only one matching word and the other had two, should they be treated the same? In other words, should two matches suggest a role is _more_ senior, or should five matches and one match be lumped together as simply a 'senior' job posting?

I therefore made matching features and ran each model below with both **ordinal** features (e.g. 3 matches equals a 'seniority' score of 3, which is considered three times as senior as a seniority score of 1) and **binary** features (senior (1) or not senior (0)). Using cross-validation, I typically found that the ordinal model had more success in classifying job roles; however, I find the arguement for using the binary model to be more compelling. For specific performance of the two models, please refer to my github.

###End of Side Note - Back to the Model


I found that using the above features, I was able to produce a model with an F1 Score of .75 (a metric that combines precision and recall), or an area under the curve of .76. This suggests my first models was indeed outperforming random chance.

However, there was a major problem: for two of my three variables, the effect was working in the opposite direction I expected. 'Big Data''s coefficient seemed to change each time I ran a new regression, while Data Skills was consistently negative. Was is plausible that knowing R or Python _hurt_ one's compensation as a Data Scientist?

Nonetheless, seniority was strongly associated with higher compensation: when I ran the model using only seniority as a feature, neither my F1 score nor area under the ROC curve decreased drastically. However, when I included Data Skills as a feature with Seniority, my model still outperformed the one-feature model, while 'Skills' still had a negative coefficient.

##Back to the Drawing Board

I revisited my "skill" and 'Big Data' list I had used to parse the job descriptions and titles in my dataset, and realized that it were more logical to restructure the lists and run the model again. First, I decided to run the test against both the description and the title, instead of just the former. I opted to create 3 new features to supplement the 'seniority' variable (with both ordinal and binary versions):

1. **Big Data (v2)** In this list, I used as specific a list of 'big data' terms as I could, to denote skills likely being used in large enterprises where the engineering challenge of processing data is as problematic as analysis itself ('hadoop', 'spark', 'hive', 'engineer', 'architect', 'scala', 'julia' etc..). I expected this would be associated with higher pay.

1. **Data Analysis/ Programming** I attempted to include just languages used in teams that analyze data or work with data. This included Python, R, SQL, Matlab, SAS and Java. I expected these would be less positively associated with compensation than the first feature.

1. **Tedious or Low-Level Tasks** This was how I accounted for roles using tools that are accessible to non-Data Sciencists, or where skills that are somewhat unspectacular (but important!) would be listed. This included cleaning, anything related to excel, keywords related to business intelligence and analytics. I imagined this would be negatively associated with higher pay.

What I found was encouraging: the first and the third features above operated in the direction I expected (positive and negatively, respectively) while the second was consistently negative when included with the seniority and big-data features.

I employed an additional Grid Search to optimize my C parameter and to choose a penalty, finding an optimal C value of 0.1 and a Lasso Regression (L1) penalty.

![Heatmap of Classification, with Associated Scores](htt**?raw=true "A Look Into Jupyter Notebook Output")

This last model was very strong in classifying my testing set (33% of data or 805 values), with an Area under the Curve of 0.94, and an average F1 score of .95. Using cross-validation on the whole dataset, I found similar figures.

Interestingly, for the first time there was a marked difference between the ordinal and binary models: the binary model actually performed less well on training data with these new features than the initial binary.
:
![Binary Model, with New Features](htt**?raw=true "A Look Into Jupyter Notebook Output")

![Ordinal Model, with New Features](htt**?raw=true "A Look Into Jupyter Notebook Output")


##Conclusion

There are two clear takeaways about employment in data science more broadly: if a given job posting uses words that denote seniority or that refer to 'big-data' tools in their job postings, it is very likely they will pay more than an a similar posting without that language (at least within the domain of postings on indeed.com).

More broadly, however, having intimate control over how data is cleaned, analyzed and interpreted is incredibly important in conducting such research, as this project offered numerous opportunities to point at spurious correlations, rather than meaningful trends within the data. I would welcome feedback on my methodology in this project from any and all readers.
