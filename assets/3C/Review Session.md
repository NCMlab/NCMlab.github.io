---
name: Review Session
layout: 3cPage
---
---
name: Review Session
layout: page
---
---
name: Review Session
layout: page
---
---
name: Review Session
layout: page
---
Please list types of variables. Continuous and Categorical -- Nominal/ordinal
What comparisons have we discussed so far?
- compare two groups
- compare two measures from one group
- compare more than two groups
- compare more than two measures from one group
- compare two continuous variables
- compare one continuous variable to any number of categorical and/or continuous variables

What needs to be normal in your data?
We keep saying the residuals "need" to be normal. The data should be normal. This is a bit confusing.

If you have data and you are modelling the mean, what are the residuals? They are the data themselves. This is why the data should be normal. But be careful, if you have groups within your data, each groups data may be normal, but their combination is not. 
- This is shown in the JAMOVI dataset.

What is your data? It is a sample. From what? What are we trying to understand?
- A population

How do we compare our sample to the population? 
We have only looked at the mean so far. What measure do we need?
We look at the standard error. How do we get the standard error?
Data --> mean --> error --> sum squared error --> standard deviation --> standard error

What do we use the standard error for?
Confidence intervals.
How do we calculate 95% confidence intervals?
- What is the equation?
What is 1.96? What distribution is it from?
- Z
What do confidence intervals around the mean tell us in words?
- It is the range in which we are 95% confident that the true population mean lies.

What if our data is not normal? It has a strong skew.
- Think about what we have done. 
- The estimation of confidence intervals is symmetric around the mean.
- If the data has a strong skew, then the population data may also have a strong skew. Therefore, using centered CI is probably not the best idea.
What can we do?
- Use the "robust" method called bootstrapping.
- It resamples the data (Takes a sample of the data with replacement. With replacement means that the same data value may come out more than once in a resample.)
- Calculate your test statistic each time you resample.
- Do this 5000 times. 
- Find the dispersion (spread) of this new distribution.
- It is the standard error of you test statistic (in this case the mean)
- It can eliminate BIAS in your CI calculations when you have skewed data.
Bootstrap is shown with my MatLab figures
## Demonstrations
Descriptives on Data
Show skew
Is Skew significantly large?
Look at QQ plot
How? Remember your Z test (1.96 is two-tailed significant at 0.05):
Split data by groups and show results.

Filter to two groups (1 and 3)

Correlate Data and Cov
Regression Cov on Data
Compare results to correlation
What does estimate value mean?

Regression Group on Data
What do the intercept and estimate mean?
Look at the means of each group.
Figure it out
Ask how we ever compared two groups before? - Yes
Compare results to independent sample t-test

Regression Group and Cov on Data
Do a full walkthrough










