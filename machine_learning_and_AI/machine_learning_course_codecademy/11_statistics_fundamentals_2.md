# Hypothesis testing


## Associations


### Two-Sample T-Test
Two sample t-test is used for association between a quantitative variable and a binary categorical variable. For example we want to test the web traffic on our page based on if it is version 1 or version 2. 
The _null hypothesis_ for the test is that the average lenght of a visit does not change based on the website version, while the alternative hypothesis is that it does

In python we can use the scipy ttest_ind(), it takes the values of each groups as inputs and returns the t-statistic and the p-value:
```py
from scipy.stats import ttest_ind
tstat, pval = ttest_ind(times_version1, times_version2)
```
If pval < 0.05 we reject null hypothesis as there is a significant difference.




### ANOVA and Tukey Tests
Are used for association between a quantitative variable and a non-binary categorical variable. (We could do combined t-tests to tests all separate associations but it'd be too time consuming as we would need to investigate all permutations, this could inflate the probability of type 1 error as we're testing more timese)


#### ANOVA
Anova (analysis of variance) tests the null hypothesis that all groups have the same population mean (eg. the true average is the same in every distribution). We can perform it in python using f_oneway(), which returns the f-statistic and the p-value
```py
from scipy.stats import f_oneway
fstat, pval = f_oneway(scores_mathematicians, scores_writers, scores_psychologists)
```
If the p-value is below our significance threshold, we can conclude that at least one pair of groups earned significantly different scores on average (rejecting null hypothesis). However, we need to investigate further to find the groups of interest.


#### Tukey's Range test
Tukey's range test complements the ANOVA test in case the p-value was significant. Turkey's test is used to find out which pair of sets are different. This can be used in python with the function pairwise_turkeyhsd(). This is similar to running 3 separaet t-tests, except that it runs all of these tests simultaneously in order to preserve the type 1 error rate. 

```py
from statsmodels.stats.multicomp import pairwise_tukeyhsd
tukey_results = pairwise_tukeyhsd(data.score, data.major, 0.05) 
#The data is given in format quantitative - categorical var
print(tukey_results)


#Prints:
Multiple Comparison of Means - Tukey HSD,FWER=0.05
==========================================
group1 group2 meandiff lower upper reject
------------------------------------------ 
  math  psych    3.32 -0.11  6.74  False 
  math  write    5.23  2.03  8.43  True
 psych  write   -2.12 -5.25  1.01  False 
------------------------------------------
```
The output is a table with one row per pair-wise comparison. Where reject is _True_, we 'reject the null hypothesis' and conclude there is a significant difference between the groups.



## Assumptions of T-tests, ANOVA and Tukey
- The observations should be indipenently sampled from the population

- The standard deviation of the groups should be equal
	+ The two-sample t-test can ignore this assumption setting ttest_ind(.... , equal_var=False)

- The data should be normally distributed
	+ (can ignore if the sample size is large)

- The groups created by the categorical variable must be independent
	+ (the groups tested cannot be a repeat of the same with different variables, and ideally shouldn't be able to influence each others)





### Chi-Square Tests
Used for association between two categorical variables. (Ex ppl under and over 40 (binary) were given a survey on 3 products (categorical), did the groups choice differ?)
We can perform the chi-square test in SciPy with the function chi2_contingency(). This takes as input a contingency table which can be created with the crosstab() function.

```py
import pandas as pd
table = pd.crosstab(variable_1, variable_2)
 
#run the test:
from scipy.stats import chi2_contingency
chi2, pval, dof, expected = chi2_contingency(table)
```
The null hypothesis is that there is no association between between the variables. If the p-value is below the threshold, we reject the null hypothesis and conclude that there is a statistically significant association between the two variables.

#### Chi square test assumptions
- The observation should be independently randomly sampled from the population

- The categories of both variables must be mutually exclusive
	+ Eg. individual obsevations should fall into one category per variable

- The groups should be independent   








































# Experimental designs

## Hypothesis testing
Summary
- Comparing a sample statistic to a hypothesizes population value for a:
	+ Quantitative variable   (One sample t-test)
	+ Categorical variable    (Binomial test)

- Testing association between binary categorical variable and a:
	+ Quantitative variable   (Two-sample T-test)
	+ Categorical variable 	(Chi-Square test)

- Testing an association between a categorical variable (>3 categories) and a:
	+ Quantitative variable   (ANOVA with Tukey's Range Test)
	+ Categorical variable    (Chi-Square test)





## A/B Testing
A/B tests usually compare an option that we're currently using to a new option that we sustpect might be better. 
This needs a metric of comparison and usually this is given by the percent of users who take a certain action upon interaction with one of the options.

*Baseline*
Baseline is the reference to the historical data for that option that we're currently considering (ex old ratio of purchase online. simple percentage)


*Minimum detectable effect*
This is to determine beforehand what's the minimum 'value' increase in taking this action, also called _desired lift_. It is usually expressed as a percent of the baseline conversion rate. (ex our baseline sales were 6% but we want to get 8%). Calculated as ((new-baseline)/baseline)* 100


*Significance threshold*
Ex we set the significance threshold at 0.05, and we reject the null hypothesis and conclude that the population b is significantly different from the population a if p<0.05. This value represents the _false positive rate_ for the test (finding a significance when there's none)
To not incur in false positive we want to increase the sample size 
We also want to increase the significance threshold for more accuracy when determining the true positive rate (but raises false positive rate)


*The power of aa test is the probability of correctly detecting a significant result*













































