# EDA (Exploratory data analysis)

EDA is finding out what is there, what patterns you can find, and what relationships exist. EDA is the important first step towards analysis and model building.

EDA works on 3 main points:
- Data inspection
	+ Inspect the data to determine data types and missing values
- Numerical summarization
	+ Numerical summaries to check what shape of data we're working with (`data.describe(include='all')`)
- Data visualisation
	+ Graphs etc.


## Types of data
There are 2 main data types - *CATEGORICAL and NUMERICAL*

_Categorical_ variables can be:
- Nominal variables
	+ which describe something
- Ordinal variables
	+ Which have an inherent ranking
- Binary variables
	+ Which have only 2 variations


_Quantitative (numerical)_ variables can be:
- Continuous
	+ For a variable to be continuous, there has to be infinitely smaller units of measurment between one unit and the next
- Discrete
	+ Discrete variables come from counting, come in units.



# Pandas object interpretation
Pandas automatically interprets the pieces of data inserted in the DF, this can be inspected with:
`df.dtypes`

The main data types are:
- int32/64 -> in columns where all values are numbers with no decimals
- float64 -> in columns where at least one value has decimals
- object -> any non numeric values appears in one column 
	+ (!! Not optimal bc we cannot use operations that the usual data type would allow) 
	+ (!!Nominal variables become objects)
- string -> characters 
	+ (Most of the times you need to set it, but worth cus you can do operations like .lower())
- bool -> boolean

*You can change the pre-assigned data types with:*
```py
df['column'] = df['column'].astype('string')
```

!! Quirk: You cannot convert between float and int if there are any null values
Replace null with 0: 
```py 
movies['cast_count'].fillna(0, inplace = True)
```


## Variables to data type

*Categorical*
- Nominal
	+ Objects
- Ordinal
	+ Category (`df['col'] = pd.Categorical(df['col'], ['down', 'mid', 'high'], ordered=True)`)
	+ Gives access to function .sort_values()
- Binary
	+ Boolean
	
*Quantitative*
- Continuous
	+ float
- Discrete
	+ int	

## One-Hot Encoding (OHE)
When we have categorical values and we want to transpose it in binary values we can do OHE. Eg. We have a column that either has [a, b, c], with this we would create a new DF with 3 more columns (a, b, c) in which each row has either 0 or 1 based on the value it had before. ex 'm' : a -> 'm': 1 0 0

Syntax:
```py
df = pd.get_dummies(data=df, columns=['Embarked'])
print(df.head())
```







# EDA Steps
- Initial data inspection
	+ inspect the dataset
	+ understand the data

- Data information
	+ Check for unique features
	+ Check for missing columns
	+ Check for appropriate data type (df.info())

- Inspect missing data
	+ check with `df[df.isnull().any(axis=1)]`





# Types of summary statistics

## Univariate statistics
It's summary statistics focussing on a single variable. It includes:

*Quantitative variables:*
- Central location (central tendency): used to communicate 'typical values'
	+ Mean
	+ Median
	+ Mode
	+ Trimmed mean (mean excluding x percent of the lowest and highest data points)
- Spread: describes variability within a feature
	+ Range (difference between max and min)
	+ Inter quartile range (difference between 75th and 25th percentile)
	+ Variance (average of the squared distance from each data point to the mean)
	+ Standard Deviation (square root of variance)
	+ Mean Absolute Deviation - MAD (mean absolute value of the distance between each data point and the mean - less impacted by extreme outliers)


*Categorical variables*
Given that these variables are not necessarily spaced like numbers we need a different approach:

- Frequency or proportion of observation in each category
```py 
df.column.value_counts() #normalize=True to get percentage values
```


## Bivariate statistics
Used to summarise the relationship between two variables

- One quantitative and one categorical:
	+ ex transmission type to price
	+ Mean or Median difference
- Two quantitative variables:
	+ ex year and selling price
	+ Correlation
- Two categorical variables
	+ ex transmission type to seller type
	+ Contingency table and Chi-Square statistic




# Summary statistics in pandas

## Univariate statistics

### Quantitative variables

In order to get univariate statistics we can simply use .describe(). If we want to get statistics for categorical variables as well we will need to include "include='all'"
```py
print(df.describe(include='all'))
```

If we only want *central statistics* specific to a column:
- Mean
	+ df.column.mean()
- Median
	+ df.x.median()
- Mode
	+ df.x.mode()
- Trimmed mean
```py
from scipy.stats import trim_mean
trim_mean(df.x, proportiontocut=0.1) #trims extreme 10%
```


If we want *spread statistics* specific to a column:
- Range
	+ df.x.max() - df.x.min()
- Interquartile range
	+ df.x.quantile(0.75) - df.x.quantile(0.25)
!or
```py
from scipy.stats import iqr
iqr(df.x)
```
- Variance
	+ df.x.var()
- Standard deviation
	+ df.x.std()
- Mean absolute deviation
	+ df.x.mad()


*Visualise quantitative variables*
We can use python's _seaborn_ library, built on top of matplotlib. It ffers the boxplot() and histoplot() functions to plot data from pandas.

```py
import matplotlib.pyplot as plt
import seaborn as sns

# Boxplot for column 'rent' in df rentals
sns.boxplot(x='rent', data=rentals)
plt.show()
plt.close()

#Histogram for same data
sns.histoplot(x='rent', data=rentals)
plt.show()
plt.close()
```



### Categorical variables*

To obtain a value count of categorical variables simply use value_counts() on the column to get all the counts of each unique variable
```py
df.x.value_counts()
```

If you want to get a proportion, simply divide by number of instances:
```py
df.x.value_counts() / len(df.x)

# Alternatively we can just add the option normalize
df.x.value_counts(normalize=True)
```


*Visualise categorical variables*
Seaborn offers options for barcharts (countplot()) and pie charts (complex):
```py
#Boxplot for column 'x' in df rentals
sns.countplot(x='x', data = rentals)
plt.show()
plt.close()



#Pie chart with the same data through pandas wrapper
rentals.x.value_counts().plot.pie()
plt.show()
plt.close()
```




# Association between quantitative and categorical variables

Get all rows respecting a variable: 
In this case from the students df we want to get the scores based on the address if its city 'C' or rural 'R'

```py
score_city = students.scores[students.address == 'C']
```

With this method we could for example find the difference in mean between the scores, or other metrics of interest.

*Show a double boxplot based on number of unique categorical variables*
```py
sns.boxplot(data=df, x='address', y='score')
plt.show()
plt.close()
```


*Plot overlapping histograms using variable-specific data*
```py
plt.hist(scores_GP , color="blue", label="GP", normed=True, alpha=0.5)
plt.hist(scores_MS , color="red", label="MS", normed=True, alpha=0.5)
plt.legend()
plt.show()
```
!!! in new version is density instead of normed, alpha is for transparency


# Association between two quantitative variables

One of the best way is to plot them against each other in a scatter plot
```py
plt.scatter(x=df.x, y=df.y)
plt.xlabel('x')
plt.ylabel('y')
plt.show()
```


*Exploring COVARIANCE*
Covariance is a summary statistic that describes the strength of a linear relationship. It can go from -inf to +inf. Positively covarying variables change similarly while negatively change inversely. Covariance of 0 indicates no linear relationship.
To calculate covariance we can use `.cov()` which would return a matrix as such
[[variance(var1), covariance],
[covariance, variance(var2)]]
```py
matrix = np.cov(df.x, df.y)
```


*Pearson CORRELATION*
Also called correlation, it's a scaled form of covariance. It measures the strength of a linear relationship, but ranges from -1 to +1 to be more interpretable. Calculate correlation:

```py
from scipy.stats import pearsonr

correlation_value, p = pearsonr(df.x, df.y)
```





# Association between two categorical variables

*Contingency tables*
Also known as two way tables or cross tabulations, are usually for summarising two categorical variables at the same time. This returns a matrix where all permutations are counted. Ex 2 variables being yes/no, we would get the count of no/no no/yes yes/no yes/yes.

```py
contingency = pd.crosstab(df.x, df.y)
```

If we divide this by the number of total observations we obtain the frequencies in %/100
```py
contingency_freq = pd.crosstab(df.x, df.y)/len(df)
```

*Marginal proportions*
Proportion of respondents in each category of a single question. (basically sum of categories after splitting them ex sum no/no and no/yes against the other two). Sum the axis to get the marginal proportions.
```py
x_marginals = contingency_freq.sum(axis=0)
print(leader_marginals)
y_marginals =  contingency_freq.sum(axis=1)
print(influence_marginals)
```


*Expected contingency table*
Given that marginal proportions are almost seen as a bias in the analysis, we can see if these bias are effectively reported in our data by multiplying the expected values and check them against the actual ones.
To calculate the expected table we can use chi2_contingency from scipy

```py
from scipy.stats import chi2_contingency 
chi2, pval, dof, expected = chi2_contingency(contingency_freq)
print(expected)
```
Once we obtain this data, we can compare it to the actual dataset count we started with (with crosstab) and the more the two diverge, the more the variables are associated.



*Chi-Square statistic*
Summarises how different the expected and the contingency tables are. It is calculated as the squared sum of the observed minus expected: chiS = sum((observed-expected)^2)

This is also the first output of chi2_contingency().































































