# Introduction to data analysis

# Data gaps
`Garbage in, garbage out` is a data-world phrase that means “our data-driven conclusions are only as strong, robust, and well-supported as the data behind them.”



# Ethical issues regarding data collection
- Consent: individuals must be informed and give their consent for the information will be collected
- Ownership: individuals collecting data must be aware that individuals have ownership over their info
- Intention: individuals must be informed about what information will be taken, stored and how will be used 
- Privacy: these info must be kept secure, especially for identifiable info



# Data types

Data, within data science, typically means a collection of organised observations. There are two types of organisations:
- Methodology: how the data was collected IMPORTANT: 
	+ STANDARDISING MEASURMENTS
	+ Validity of data (data must be tailored to question)
- Shape: usually spreadsheet or table. Divided in variables (columns) and individual observations (rows)


Data types:
- Numerical
- Categorical
	+ Nominal (ex. species)
	+ Dichotomous (ex yes or no)
	+ Ordinal (ex how good 1-5)




# Descriptive statistics

To describe CATEGORICAL variables usually we refer to
- Frequency: count of category in dataset
- Proportion: frequency divided by total number in dataset
- Percentage: proportion times 100 


To describe NUMERICAL variables usually we describe their distributions (histogram, x=distribution, y=frequency). We usually refer to
- Mean: aka average, describes the centre of the distribution
- Standard deviation: describes the spread of the values by measuring the average distance of the values from the mean

When the distributions are SKEWED we refer instead to 
- Median: (or 50th percentile), describes the middle value of the distribution
- Interquartile range (IQR): description of values at 25%(Q1)-50%(Q2)-75%(Q3) of the distribution. The IQR is the difference between Q3 and Q1, marking the range for the middle 50% of the data. Very useful when dealing with outliers
- Mode: value with highest frequency. Not always useful but used when there are bimodal distributions



Data aggregation:
It is a way of exploring variable relationships. Usually represented by a scatter plot, plotting two variables and with each point representing an observation. Using a scatterplot we can gauge a CORRELATION COEFFICIENT in our data. This number ranges from -1 to +1 and gives us insight on our relationship in the form of:
- Direction: positive coefficient means that higher values in a variable are associated to higher value in the other, while negative represents higher values associated with lower
- Strenght: the farther the coefficient is from 0, the stronger the relationship is.





-------------

# Data visualisation

## Charts

Univariate charts:
Help visualise change in one variable. Bar Chart - Histogram - Map - Box Plot

Bi- and Multivariate charts:
To show relaionship between two or more variables. Scatter Plot - Line Chart - Bivariate map (geographical map + variable)


## Aesthetic
Information redundancy: helps keyt data points to stand out. For example in a scatter plot we can give according size to the dots to highlight the differences further.


## Notes
Log scaling!
Color scaling as in sequential (light blue to blue) - diverging (red to blue) - categorical (completely different)
Labels and titles!!



-------


# Data analysis and conclusions

Types: descriptive - exploratory - inferential - causal - predictive 

- Descriptive: or summary statistic is used to describe, summarise and visualise data so that patterns can emerge. Usually include measures of central tendency (mean, median, mode) and spread (range, quartiles, variance, standard deviation, distribution).

- Exploratory: is the next step after descriptive analysis. In this we look for relationships between variables in our dataset. Usually represented by bi- and multivariate charts. Usually includes metrics such as R squared. (cannot determine causation).
	+ Clustering: is a `unsupervised machine learning technique` useful for exploratory analysis. This techniques scouts for patterns in data that has not yet been classified. One of these techniques is PCA (principal component analysis), after which we can use k-means clustering to get differentiation in population.

- Inferential: let us test hypothesis on a sample of a population and then extend our conclusion to the whole population. For example A/B tests, where half the sample is given a certain condition and the other another and then is tested for differences in choice in the populations. We then need to calculate the probability it happened by chance eg alpha value

- Causal: causal analysis is conducted to obtain a cause-effect result in our analysis. Usually relies on designed experiment but can also be inferred using observational data. Usually these include multiple repeats as well as randomisation and a control group.

- Predictive: uses `supervised machine learning techniques` to identify the likelyhoood of future outcomes. These include regression models, support vector machines and deep learning convolutional neural networks. Requires training data and a set of already-classified data from which the algorithm can learn. Careful judging the risks that such analysis involves when making decisions.


