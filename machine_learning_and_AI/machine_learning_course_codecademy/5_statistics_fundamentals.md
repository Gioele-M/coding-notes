# Statistics fundamentals

## Probability
Probability is a branch of mathematics that allows us to quantify uncertainty, this includes set theory. 

Set theory: branch of mathematics based around the concept of sets (as in the data structure where each element is distinct and has no particular order). Set is declared as x={1, 2, 3} or with Set()

Experiment: something that produces observations with some level of uncertainty.

Law of large numbers: The more we repeat an experiment, the more the observed proportion of times each event occurs will converge to its true probability.


Concepts: *Union, intersection and complement*
- Union: union of two sets, including any element that exists in either one or both of them.
- Intersection: intersection encompasses any element that exists in both of the sets (in py x.intersection(y))
- Complement: consists of all possible outcomes outside of the set.


Concepts: *Independence and dependence*
- Independence: two events are independent if the occurrence of one does not affect the probability of the other (ex coin flip)
- Dependence: opposite, dependent if they affect the probability of the other (ex chances of getting a red rock from a pool of 5 rocks after 3 were already extracted)
- *Mutually exclusive:* two events are mutually exclusive if they caanot occur at the same time (ex chances to get head and tail on coin toss)



#### Sum of random probability

*For mutually exclusive events*
`P(A or B) = P(A) + P(B)` -> probability of A or B happening is equal to probability of A plus probability of B

*For non mutually exclusive events*
`P(A or B) = P(A) + P(B) - P(A and B)` -> need to subtract intersection of A and B otherwise it would be repeated twice given the overlap!!


#### Sum of conditional probability
The probability of one event occurring, given that another one has already occurred. Using the example of the marbles, where in a bag are present 3 red and 2 blue marbles (total 5)

*Probability of getting a red marble after the first one extracted was blue*
`P(Red Second | Blue First) = 3/4`

This is only in case the marbles are not replaced, otherwise the probability does not vary
`P(A|B) = P(A) and P(B|A) = P(B)`



## Multiplication rule
When calculating the probability that two events happen simultaneously:

*For dependent events:*
`P(A and B) = P(A) * P(B|A)` (well represented by a tree diagram)

*For independent events:*
`P(A and B) = P(A) * P(B)` (because P(B|A)=P(B))


## Bayes' Theorem
(not sure what is used for but in the example it calculates the prbability of having a sore throat when tested positive for it, the formula is)

`P(B|A) = (P(A|B)*P(B))/P(A)`






# Probability distributions

## Random variables
A _random_ variable is considered a function, used to represent random events. For example a random variable representing the outcome of a die roll is any number between one and six. Random variables must be numeric.

*Random variables in python*
```py
random_var = np.random.choice(a, size=size, replace=True/False)
# a = list or object with values that can be sampled
# size = how many numbers to choose
# replace = if to keep the value in a after drawing it (True keeps it, false removes it)
```


### Discrete and continuous random variables

Discrete random variables are variables with a countable number of possible values.
Continuous random variables are variables with an uncountable number of values, eg measurements



### Probability Mass Function

PMF is a type of probability distribution that defines the probability of observing a particular value of a *discrete* random variable. (ex PMF can be used to calculate the probability of rolling a 3 on a die)

*Parameters in the PMF formula:*
- n -> for the number of trials (eg 10 if we flip the coin 10 times)
- p -> for the probability of success in each trial (eg 0.5 bc is a coin)
- x -> the value of interest (eg how many times we want to see that happen)

```py
import scipy.stats as stats

#(x, n , p)
print(stats.binom.pmf(6, 10, 0.5))

#prints 0.205078 -> which is the probability of obtaining 6 heads tossing a coin 10 times.
```

*PMF over a range*
For discrete random variables the PMF over a range is simply calculated by summing the probability of the components of the range. (P1+P2...) (ex odd number when shooting a dice)
In python you have to use the stats.binom.pmf() to sum individually the results. IF YOU HAVE TO CALCULATE SOMETHING LIKE 8 out of 10 values, you can just calculate the remaining two and do `1 - (calc(val1) + calc(val2))`

```py
# less than or equal to 8
val = 1 - (stats.binom.pmf(9, n=10, p=.5) + stats.binom.pmf(10, n=10, p=.5))
```




### Cumulative distribution function

CDF is similar to the probability mass function, however it calculates the probability of obtaining a specific value OR LESS. Cumulative distribution functions are constantly increasing, so for two different numbers that the random variable could take on, the larger nu,ber will always have greater probability.

It can be used to calculate the probability of a specific range by taking the difference between two values from the cumulative distribution function!!

```py
import scipy.stats as stats

#(x, n, p)
print(stats.binom.cdf(6, 10, 0.5))

#prints 0.828125 -> probability of obtaining 6 heads OR LESS!!

# To calculate a range just subtract two ranges! m < x < n do: cdf(n) - cdf(m)

# To calculate a range of a number OR MORE, just do the (1 - cdf(n))
```




### Probability density functions

Probability density functions are used for *continuous random variables*, and span across all possible values that the given random variable can take on. 
On graph it is a standard curve across all values with a total area under the curve of 1.

With this type of distribution we can never calculate the probability of a single point, we have to use the cumulative distribution function for the given probability distribution!

Being the probability distribution a _normal distribution_, the parameters for it are the *mean and SD*.
After determining the shape of the curve with mean and SD, we can use the cumulative distribution function to calculate the area under the probability density function curve to find the effective probability.

The function to calculate this takes 3 parameters:
- x: the value of interest
- loc: the mean of the probability distribution
- scale: the standard deviation of the probability distribution

!!! Being this calculated with the cumulative distribution function, it calculates the probability of obtaining x OR LESS!!

```py
import scipy.stats as stats

# cdf(x, loc, scale)
print(stats.norm.cdf(158, 167.64, 8))

#prints 0.1141
```

*To calculate a range of values* we can simply calculate the difference between the two cumulative probabilities as we would for binomial distributions!


*To calculate the probability of a value OR MORE* we can simply do (1 - cdf(x))






# The Poisson Distribution
The Poisson distribution is used to describe the number of times a certain event occurs within a certain time or space interval. 
For example it can be used to describe the number of cars that pass through a specific intersection within 4 and 5pm. 
The Poisson Distribution is defined by the rate parameter (or expected parameter) symbolised by the Greek letter lambda.
Lambda represents the expected (or average) value of the distribution. (LAMBDA WHEN FED AS PARAMETER REPRESENTS THE AVERAGE VALUE OF THE DISTRIBUTION WHICH IS GOING TO BE IN THE MIDDLE OF THE GRAPH AND CURVE)

!!In the Poisson distribution, the variance is also the value lambda, meaning that the expected value and variance are equivalent. This means that the larger the expected value is, the more evenly spread the distribution is; this is also bc observations start from 0, therefore low expected values skew the curve slightly (bc you can't have -1 customers in the shop, so that 'tail of distribution gets distributed')

(The variance of binomial distribution is n * p * (1-p))


### Calculating probabilities of exact values using:
### Probability Mass Function (PMF) on the Poisson distribution

Being the Poisson distribution a discrete probability distribution, it can be described by a probability mass function and cumulative distribution function.

Using `stats.poisson.pmf()` we can calculate the probability of observing a specific number given the expected value of a distribution. Ex. we expect it to rain 10 times in the next 30 days; the number of times it rains in the next 30 days is 'Poisson distributed' with lambda = 10, after which we can compute what's the probability of observing a different value ex 6.

```py
import scipy.stats as stats

#Expected value (lambda) = 10, probability of observing 6 instead
stats.poisson.pmf(6, 10)

# prints
# 0.0630..

```
Once again, to calculate ranges of probabilities, simply sum the probabilities



### Calculating probabilities of a range using:
### Cumulative density function on the Poisson distribution

We can use `poisson.cdf()` to evaluate the probability of observing a specific number or less given the expected value f the distribution. Ex we want to calculate the probability of observing 6 OR LESS rain events in the next 30 days wen we expected 10:

```py
import scipy.stats as stats

# expected = 10, probability = 6
stats.poisson.cdf(6,10)

# prints
# 0.1301...
```
As always if we want to calculate ranges or MORE we do differences and 1-(n-1)









# Expectation and variance
In the Poisson distribution the lambda rapresents both the expected value and the spread of the curve, making it so that a larger expected value results in a better spread and less skewed curve.

In the binomial distribution the expected value is equal to n_attempts * probability = n * p, while the variance is calculated as n_attempts * probability * (1-probability) = n * p * (1-p)

### Properties of expectation
- The expected value of two independent random variables is the sum of each expected value 
	+ E(X+Y) = E(X) + E(Y)

- Multiplying a variable by a contant 'a' changes the expected value to be 'a' times the expected value of the random variable. Eg if we know that we get 5 heads on 10 flips, we know we will get 5 * 4 = 20 heads on 10 * 4 = 40 flips
	+ E(aX) = aE(X)

- Adding a constant 'a' to a distribution changes the expected value by the value 'a'. Ex if the teacher gives everyone 2 extra marks, now the average will be 2 points higher than it should
	+ E(X+a) = E(X) + a



### Properties of variance
- Increasing the values in a distribution by a constant 'a' does not change the variance. (it changes the mean lol)
	+ Var(X+a) = Var(X)

- Scaling the values of a random variable by a constant 'a', scales the variance by the constant squared
	+ Var(aX) = a^2Var(X)

- The variance of the sum of two random variables is the sum of the individual variances (ONLY IF X and Y are independent random variables!)
	+ Var(X+Y) = Var(X) + Var(Y)












# Sampling from a population
Sampling is useful to gain insight on the data and so not to analyse it all, on python we can use numpy.random.choice() which generates a sample of some size from a given array

```py
import numpy as np

sample = np.random.choice(np.array(my_array), sample_size, replace = False) 
# sample_size is an integer and replace 
```

### Central Limit Theorem
The Central Limit Theorem (CLT) states that the sampling distribution of the mean is normally distributed as long as the population is not too skewed or the sample size is large enough (usually n > 30). 

The CLT not only establishes that the sampling distribution will be normally distributed, but it also allows us to describe the normal distribution quantitatively via:
- mean µ (mu)
	+ We take the sample size _n_ from a population (with true mean _µ_ and SD _sig_) and calculate the sample mean _x_. If _n_ is sufficiently large (n>30), the sampling distribution of the means will approximately equal.
- standard deviation (sigma) (_sig_)
	+ In the case that x is respected, the sampling SD equals to the population SD divided by the square root of the sample size => Sampling distribution SD = _sig_ /√n

_Second part_
The sampling distribution of the mean is normally distributed, with standard deviation equal to the population standard deviation (_sig_) divided by the square root of the sample size n. SampleDistribution SD = _sig_/√n
The standard distribution SD is also known as the _STANDARD ERROR OF THE ESTIMATE OF THE MEAN_. 
In many instances we cannot know the population standard deviation, so we estimate the standard error using the sample standard deviation: _Standard Error_ = SampleDistribution SD / √n
To note:
- As the sample increases, the standard error descreases
- As the population SD increases, so will the standard error



### Biased/Unbiased estimators
An unbiased estimator is a statistical parameter which aligns properly between the actual population and a sample, for example the mean. A biased estimator instead wouldn't be as reliable, for example a maximum or minimum value of the population


### Calculating probabilities
Once we know the sampling distribution of the mean, we can use it to estimate the probability of observing a particular range of sample means, given some information about the population, using the *Cumulative Distribution Function (CDF)*
Ex. We want to calculate the mean weight of 10 salmons. We know the average weight and SD. We can use the probability distribution that 10 random sampled fish will have the mean weight less than or equal to 75:

```py
x = 75
mean = 60
std_dev = 40
samp_size = 10
standard_error = std_dev / (samp_size**.5)
#  **.5 is raising to the power of one half, or square root

stats.norm.cdf(x,mean,standard_error)
```












# Inferential statistics
Descriptive and inferential statistics are two subfields of statistics. Descriptive statistics produces numerical and visual summaries of the data. Inferential statistics instead is used to draw inferences about a population using a smaller sample of data, basically hypothesis testing


## Hypothesis testiing - One Sample T-Test

Steps:
- Ask a question:
	+ What type of trend are you looking for? Is our population in line with what we expected

- Define the Null and Alternative hypoteses
	+ Hypothesis 1 (or null hypothesis)
		* (Given that our sample's mean is higher than other populaitons) Our sample is coming from the actual population, and we simply sampled a group wiht higher scores
	+ Hypothesis 2 (or Alternative hypothesis)
		* Our sample is coming from people more prepared and that's why the average is higher
		
- Generate a null distribution
	+ The distribution across repeated samples of the statistics we are interested in if the null hypothesis is true
	+ Basically we sample from the general population (the one we're checking against), and recreate the distribution using the CLT. We then check how our population average compares to the general population distribution

- Calculate a P-Value (or Confidence Interval)
	+ The question asked is, given that the null hypothesis is true, how likely is it that their average scores is that much more?
	+ We can approach this in 3 ways:
		* Option 1
			- The alternative hypothesis is that our sample came from a population with average score higher than the actual population.
			- This would give a _p-value_ showing how probable it is (in %/100) (this value would include the right tail of the distribution where the values are higher, for this example 0.031)
			- !! Referred to as one-tailed test
		* Option 2
			- The alternative hypothesis is that our sample came from a population with an average score that is not equal to the general population (both greater or less than)
			- In the previous example we were observing a sample average that was at least so much HIGHER than the population average. For this example we are interested in the probability of observing a sample average that is at least so much DIFFERENT (higher OR lower) than the average. This example would include both tails of the distribution in our _p-value_, giving a p of 0.062 (double of whart we had)
			- !! Referred to as two-tailed test (often default test)
		* Option 3
			- Alternative hypothesis is that our sample came from a population with an average score that is less than the general population
			- This example was made from completeness, given that it's hard to sample lower-performing data points and obtain a higher average. In this case the p-value is 0.969

- Interpret the results
	+ Considering option 1 hypothesis (pvalue 0.031)
		* This means that if our population was randomly selected from the full population, there is a 3.1% chance of their average score being so much higher. This means that is relatively unlikely that we sampled our population from the general one and invalidates the null hypothesis. Therefore, it's very likely our population had some underlying advantage or variable in their favour.
		* In this case we accept our alternative hypothesis (keeping in mind that we didn't test for it, but the opposite only!)
	+ Significance threshold
		* p-value si considered _significant_ if the threshold is under 0.05 in general.
	+ The alternative hypothesis influences the result of the analyisis, given that the two tailed test produced non significant results!



## One sample T-Test in Python
T-Tests are used to compare a _sample_ average to a _hypothetical population_ average. One sample t-test can be used to address questions such as:
- Is the average amount of time that visitors spend on a website different from 5 minutes?
- Is the average amount of money that customer spend on a purchase more than 10usd?

In this example we want to see if the average buy in our website is much different from 1000$:
- Null hypothesis: the average cost of an order is 1000$
- Alternative hypothesis: the average cost of an order is NOT 1000$

*We can use the SciPy function ttest_1samp()*, this function takes in:
- Sample distribution: eg an array with our observations, this is used to determine the sample size and estimate the amount of variation in the population. This is used to estimate the null distribution and calculate the pvalue
- Expected mean: in this case 1000 as we were expecting the average buy to be 1000%

It returns
- T-statistic: (low indicates that the group are similar, high indicates difference)
- P-Value

```py
from scipy.stats import ttest_1samp

tstat, pval = ttest_1samp(sample_distribution, expected_mean)
```


### T-Test assumptions
- The sample was randomly selected from the population
	+ (ex if you only collect data from visitors that allow it that is not randomly selected)
- The individual observations were independent
	+ (ex a customer convinces a friend to buy his favourite product, that choice was influenced hence not independent)
- The data is normally distributed without outliers OR the sample size is large enough
	+ Common accepted size is more than 40. For smaller sizes is good to plot the distribution and make sure it's not skewed, in which case it would be best picking a different test






# Binomial Test
Binomial tests are useful for comparing the frequency of some outcome in a sample to the expected frequency of that outcome. For example if we expect 90% of ticketed passengers to show up for their flight, but only 80 of 100 ticketed passengers actually show up, we could use a binomial test to understand whether 80 is significantly different from 90.
Binomial tests are similar to one-sample T-tests in that they test a sample statistic against some population-level expectation, the differences are:
- Binomial tests are used for binary categorical data to compare a sample frequency to an expected population-level probability
- One sample t-tests are used for quantitative data to compare a sample mean to an expected population mean.

Example: calculate if the rate of online sale this month is less
Binomial tests use:
- Size of the tested sample
- Size of the tested sample that bought an item (observation)
- Expected size to be buying an item (expectation)


After calculating the null distribution of our sample, we can calculate an interval covering 95% of the values of our distribution `np.percentile(outcomes, [2.5, 97.5])`. This is the *95% Confidence Interval*, if our expected value falls within the 95%CI, it is reasonable to assume that our observation comes from the population distribution. (still reasonable that it was not)


*One sided p-value*
Ex we throw a coin 10 times, how likely it is to hit 2 or FEWER heads?
- Null: probability is 0.5
- Alternative: probability is less than 0.5
We check our distribution and only consider values between 0 and 2, obtaining a one-sided p-value


*Two sided p-value* (usually default)
Ex. we throw a coin 10 times, how likely it is to hit 2 or fewer, or 8 or higher


### SciPy binomial test
`binom_test` (two sided by default, can be change with the `alternative` parameter eg alternative='less')
Inputs:
- Number of observed success
- Number of total trials
- Expected probability of success
It returns the probability of obtaining the observed success based on the expected probability and number of trials.

```py
from scipy import binom_test

p_value = binom_test(2, n=10, p=0.5)

#output 0.109
```
The result indicates that obtaining fewer than 2 heads OR more than 8 is 10.9%





# Significance thresholds
P-value of 0.05 means that there is a 5% chance of observing the null hypothesis, you therefore reject it.

### Error types

- Type 1 error: false positive -> p-value is significant but the null hypothesis is true, so we reject a true hypothesis (with an alpha of 0.05 we have 5% of this happening)
- Type 2 error: false negative -> p-value is not significant but the null hypothesis is false, so we don't reject a false hypothesis


Need to keep in mind that the possibility of a type 1 error keeps stacking up (p1 * p2 * p3 ...), so if you test multiple hypothesis at once you better lower alpha








# Linear regression
Linear regration is a modelin technique that can be used to understand the realationship between a quantitative variable and one or more other variables, often with the goal of making predictions.
ex. What's the relationship between apartment size and rental price for NYC apartments?

Linear regression involves fitting a line to a set of datapoints. The equation of a line is `y = mx + b`
- x, y -> represent variables such as height and weight
- b -> represent the y-intercept of the line
- m -> represents he slope

A common choice for linear regreation is the Ordinary Least Squares (OLS) method. This defines the 'best' line as the toal squared error for all data points. The total squared error is called a _loss function_ in machine learning. (The squared error is the squared of the y-distance from the line)

*In python using statsmodels.api*
```py
import statsmodels.api as sm

# We want to predict column_1 using column_2 as a predictor
model = sm.OLS.from_formula('column_1 ~ column_2', data=pandas_df)
results = model.fit()
print(results.params)
# output:
# interceipt (b) in[0], height (m) in[1]

#We can also use it to predict 
print(results.predict({'column_2':[x]})) #where x is an numerical value

```


## Assumptions of linear regression
- The relationship between the outcome variable and the predictor is linear
(residuals are the difference between fitted values and actual values)(fitted values are the prediction of the column_1 values made on column_2 and the model)

- Normality assumption: states that the residuals should be normally distributed (to check this assumption we have to inspect an histogram of the residuals and make sure the distribution is approximately normal)

- Homoscedasticity assumption: states that the residuals have equal variation across all values of the predictor variable. (made by plotting the residuals against the fotted values in a scatter plot. If assumption is met the prlot looks like a random splatter of point centered around y=0. If there are patterns or asymmetry it would mean that the assamption is not met and the regression may not be appropriate)



### Categorical predictors
So far we used quantitative predictors but we can also use categorical predictors such as a binary variable (simplest)

```py
model = sm.OLS.from_formula('numerical ~ categorical_predictor', data)
results = model.fit()
print(results.params)

# Interceipt is the expected value of the outcome variable when the predictor is 0
# Slope is the expected difference in the outcome variable for a one-unit difference in the predictor variable
```












