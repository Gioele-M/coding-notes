# Data visualisation

## General commands for all graphs
```py
plt.xlabel(x) #-> add x label 
plt.ylabel(x) #-> add y label

plt.title(x) #-> add title
plt.legend([x, y]) #-> adds legend following the order of the array
# plt.legend also takes in keyword argument 'loc', which equals to an integer for positioning (ex 0 is best position, 1 is upper right etc)

plt.close('all') # -> to close all existing plots before starting new ones


#Create a plot of specific length (inch) and save it 
plt.figure(figsize=(4, 10)) 
plt.plot(x, parabola)
plt.savefig('tall_and_narrow.png')

```


### Subplots
If we want to display two or more graphs at the same time we can use the function plt.subplot() (which creates a _figure_ object containing all graphs). Subplot takes in 3 arguments, the number of wors of subplots, the number of columns and the index of the current subplot

```py
# First Subplot
plt.subplot(1, 2, 1)
plt.plot(x, y, color='green')
plt.title('First Subplot')
 
# Second Subplot
plt.subplot(1, 2, 2)
plt.plot(x, y, color='steelblue')
plt.title('Second Subplot')
 
## !! adjust positioning
plt.subplot_adjust(wspace=0.35)

# Display both subplots
plt.show()
```

!In case our subplots were to overlap, we can simply use `plt.subplots_adjust()`, it takes in keyword arguments:
- left: left side margin (default 0.125)
- right side margin (default 0.9)
- bottom: bottom margin (def 0.1)
- top: top margin (def 0.9)
- wspace: horizontal space between adjacent subplots (def 0.2)
- hspace: vertical space between adjacent subplots (def 0.2)


### Subplot-related commands

*Modify tick marks*
```py
# We need to create an axis object before being able to interact with the tick marks, ex

ax = plt.subplot()
plt.plot(x, y)
plt.plot(m, n)

ax.set_xticks([1, 2, 4])
ax.set_xticklabels(['10%', '20%', '40%'])

ax.set_yticks([0.1, 0.6, 0.8])
ax.set_yticklabels(['10%', '60%', '80%'])
```








## Line graphs

```py
from matplotlib import pyplot as plt

plt.plot(x_values, y_values)

#If you want to add anther line call again plt.plot before plt.show
# To give line colour, marker and line style:
plt.plot(x_values, y_values, color='green', linestyle='--', marker='*')
plt.show()
```

*Add error bars*
Or in this case fill between, it takes in 3 arguments
- x-values: same as the ones you'd give the plot
- lower bound for y values: sets the bottom of the shaded area (y values - error)
- upper bound for y values: sets the top of the shaded area (y values + error)
- ! alpha: sets the level of transparency

```py
plt.fill_between(x_values, y_lower, y_upper, alpha=0.2) #this is the shaded error
plt.plot(x_values, y_values) #this is the line itself
plt.show()
```



## Bar chart
(For categorical variables)
*Simple bar chart*
```py
plt.bar(x_values, y_values)
plt.show()
```

*Side by side bars*
In order to do this you have to follow a little formula dictating the width of our bars
```py

def adjust_plot(n, t, d, w):
	return [t*element + w*n for element in range(d)]

n = 1  # This is our first dataset (out of 2)
t = 2 # Number of datasets
d = 7 # Number of sets of bars
w = 0.8 # Width of each bar

x_values1 = adjust_plot(n, t, d, w)
x_values2 = adjust_plot(m, y, f, e)


plt.bar(x_values1, y_values1)
plt.bar(x_values2, y_values2)

plt.show()
```

*Stacked bars*
You need to use keyword 'bottom' to make the stacking bars sit on top of the others
```py

#Bottom bars
plt.bar(x, y)

plt.bar(x2, y2, bottom=y) 

plt.show()
```


*Add error bars*
```py

yerr=2 # yerr is the size of the error bar
capsize=10 # capsize is the width of the error bar arms

plt.bar(x, y, yerr=yerr, capsize=capsize)

plt.show()
```



#### Bar charts on Seaborn
(Built off matplotlib library)
```py
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

flu_results = pd.read_csv("flu.csv")
sns.countplot(flu_results["Results"])

#If you want to add an order to the data on the x axis add the order keyword with .value_counts(ascending=True).index
sns.countplot(df["victory_status"], order=df["victory_status"].value_counts(ascending=True).index)

#Otherwise simply give an order yourself as list
sns.countplot(df["Grade Level"], order=["First Year", "Second Year", "Third Year", "Fourth Year"])


plt.show()
```







## Pie chart

```py
plt.pie(data) # as in array of values

#In case you don't like the tilted version that is default, you can make it flat with
plt.axis('equal')

plt.show()
```

*Legend*
Pie charts can get the legend with both plt.legend and as a keyword argument when making the plot. This second method takes in an array of strings as 'labels' keyword argument, and allows to set the % of slice occupied by each slice with 'autopct'. 'autopct' takes in a regular expression to determine the style of display of the numbers. ex.
'%0.2f' — 2 decimal places, like 4.08
'%0.2f%%' — 2 decimal places, but with a percent sign at the end, like 4.08%
'%d%%' — rounded to the nearest int and with a percent sign at the end, like 4%.

```py
plt.pie(budget_data, labels=budget_categories, autopct='%0.1f%%')
plt.show()
```



## Histogram
(For numerical variables)
```py
plt.hist(data) # as in array of values
plt.show()
```

The histogram can also be personalised with
- range=(x,y) -> takes in a tuple, sets the range to be displayed
- bins=x -> takes in an integer, defines in how many bars the data should be split (def 10)


*Multiple histograms*
This can be achieved fairly easy, you just need to plot two histograms while giving it alpha=0.5 for transparency.
If we have different numbers in the two dataset, it's best normalising the two with normed=True
```py
plt.hist(a, range=(55, 75), bins=20, alpha=0.5, normed=True)
plt.hist(b, range=(55, 75), bins=20, alpha=0.5, normed=True)

plt.show()
```






# Analyisis steps

- Univariate analysis
	+ Quantitative vars
		* Box plots - violin plots
		* Histograms
	+ Categorical variables
		* Bar plot
		* Pie chart

- Bivariate analysis
	+ One quantitative and one categorical variable
		* Box plot
		* Histogram
	+ Two quantitative variables
		* Scatter plot
	+ Two categorical variables
		* Bar plot (countplot())

- Multivatiate analysis
	+ Scatter plot
	+ Grouped box plots
	+ HeatMap
```py
# Define the colormap which maps the data values to the color space defined with the diverging_palette method  
colors = sns.diverging_palette(150, 275, s=80, l=55, n=9, as_cmap=True)
 
# Create heatmap using the .corr method on df, set colormap to cmap
sns.heatmap(rentals.corr(), center=0, cmap=colors, robust=True)
plt.show()
```

# Multivariate analysis

*Scatter plots with visual cues*
ex
```py
# import libraries
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt 
 
# load in health data
health_data = pd.read_csv('life_expectancy_data.csv')

# Hue is the third variable which determines the colour of the dots
sns.scatterplot(x = 'Schooling', y = 'LifeExpectancy', hue = 'Status', palette = 'bright', data = health_data)

# !! can also add * style='Column' * to give the dots a particular shape based on the categorical variable

plt.show()
```

*Grouped box plots*

```py
# load in salary data
salary_data = pd.read_csv('survey_data.csv')

# side-by-side box plots grouped by gender
sns.boxplot(x = "Education", y = "CompTotal", hue = "Gender", palette = "pastel", data = salary_data)
plt.show()
```



*Multi dimensional plots*

*3D scatter plot*
```py
# import library
import plotly.express as px
 
# load in iris data
df = px.data.iris()
 
# create 3D scatter plot
fig = px.scatter_3d(df, x='sepal_length', y='sepal_width', z='petal_width', color='species')
fig.show()
```





# Time series visualisation
Data represented in a single point in time is known as cross-sectional data. Data collected over period of time is called _series-data_

## Line plot
Time is usually on the x-axis and the observation on the y-axis
ex.
```py
# import libraries
import pandas as pd 
import matplotlib.pyplot as plt
import seaborn as sns
 
# load in data
sales_data = pd.read_csv("sales_data.csv")

# convert string to datetime64
sales_data["date"] = sales_data["date"].apply(pd.to_datetime)
sales_data.set_index("date", inplace=True)
 
# create line plot of sales data
plt.plot(sales_data["date"], sales_data["sales"])
plt.xlabel("Date")
plt.ylabel("Sales (USD)")
plt.show()

```


## Box plot
Time in here is also on the x axis
More useful to see time intervals
```py
# extract year from date column
sales_data["year"] = sales_data["date"].dt.year
 
# box plot grouped by year
sns.boxplot(data=sales_data, x="year", y="sales")
plt.show()
```



## HeatMap
Useful to compare observations between time intervals. In the example the heatmap represents months in the x-axis and year on the y-axis, with sales over time dictating the tone.

```py
# calculate total sales for each month
sales = sales_data.groupby(["year", "month"]).sum()
 
# re-format the data for the heat-map
sales_month_year = sales.reset_index().pivot(index="year", columns="month", values="sales")
 
# create heatmap
sns.heatmap(sales_month_year, cbar_kws={"label": "Total Sales"})
plt.title("Sales Over Time")
plt.xlabel("Month")
plt.ylabel("Year")
plt.show()
```


## Lag scatter plot
To visualise the replationship between an observation and a lag of that observation. Pandas has a lag_plot function which plots the observation at time T on the x-axis and the lag 1 observation T+1 on the y axis
```py
# import lag_plot function
from pandas.plotting import lag_plot
 
# lag scatter plot
lag_plot(sales_data)
plt.show()
```


## Autocorrelation plot
AutoCorrelation plot is used to show whether the elements of a time series are positively-negatively-indipendent correlated. There is a function for this too
```py
from pandas.plotting import autocorrelation_plot
 
# autocorrelation plot
autocorrelation_plot(sales_data)
plt.show()
```


## Log transformation
```py
log_price = housing.price[housing.price>0]
log_price = np.log(log_price)
sns.displot(log_price)
plt.xlabel('log price')
```






































