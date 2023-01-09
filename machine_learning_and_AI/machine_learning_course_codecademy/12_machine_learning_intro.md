# Machine learning

In machine learning we want the program to learn itself the rules that describes our data best, by finding patterns in what we know and applying those patterns to what we don't know.

*Supervised learning*
Supervised learning is where the data is labelled and the program learns to predict the utput from the input data. Can be further grouped in
- Regression
	+ In regression problems we're trying to predict a continuous-valued input
- Classification
	+ In classification we're trying to predict a discrete number of values (ex is this picture a human or dog)


*Unsupervised Learning*
Unsupervised learning is a type of machine learning where the program learns the inherent structure of the data based on unlabeled examples. Can be grouped in:
- Clustering
	+ Approach that finds patterns and structures in unlabeled data by grouping them into clusters



## EDA
Exploratory data analysis (EDA) is an important aspect to conduct in order to check assumptions, inspect data for anomalies and inform feature selection.





## Feature engineering
Feature: a feature is a measurable property in a dataset, and a feature can be used as an input to a machine learning model.
Feature engineering is an umbrella term for the techniques we use to help us make decisions about features in machine learning model implementations.
Implementations to take into considerations:
- Performance
	+ With an eye on the degree of accuracy
- Runtime
- Interpretability
- Generalizabilty

Feature engineering techniques:
- Feature transformation methods:
	+ These involve numerical transformations methods and ways to encode non-numerical variables. These are applied _before_ implementing the machine larning model. Include: scaling, binning, logarithmic transformation, hashing and one hot encoding. (improve performance, runtime and interpretability)

- Dimensionality reduction methods
	+ This refers to the number of features within a dataset. Reducing dimensionality allows for faster runtimes and better performance. Dimensionality reduction methods are usually machine learning algorithms themselves such as PCA, linar discriminant analyisis (LDA) etc. Lack of interpretability is one of the drawbacks.

- Feature selection methods
	+ These are to choose among the features available. Unlike dimensionality reduction, these retain the features as they are, which makes them highly interpretable. Divided in:
		* Filter methods: techniques used to 'filter' out useful features. These include correlation coefficients (Pearson, Spearman, etc), chi-squared, ANOVA and mutual information calculations
		* Wrapper methods: techniques used to search for the best set of features by using a 'greedy search strategy'. They pick a subset of features, train a model, evaluate it, try a different subset of features, train the model again, and so on until the best possible set of features and the most optimal performance is reached. These include Forward Feature Selection, Backward Feature Elimination and Sequential Floating
		* Embedded methods: these techniques are implemented _during_ the model implementation step. These techniques (such as Lasso or Ridge) tweak the model to get it to generalise better to new and unknown data. Other methods such as tree-based feature importance, provide insight into the features that are most relevant to the outcome variable while fitting decision trees to the data.
























# Numerical transformation introduction
The process of take a numerical data and change it into another numerical value. This is meant to change the scal of our values or even adjut the skewness of our data. The main ones are
- Centering
- Standard scaler
- Min and max scaler
- Binning
- Log transformations


*Centering of data*
Data centering involves subtractng the mean of the data set from each data point so that the new mean is 0. This helps us understand how far above or below each of our data points is from the mean

```py
#get the mean of your feature
mean_dis = np.mean(distance)
 
#take our distance array and subtract the mean_dis, this will create a new series with the results
centered_dis = distance - mean_dis
 
#visualize your new list
plt.hist(centered_dis, bins = 5, color = 'g')
 
#label our visual
plt.title('Starbucks Distance Data Centered')
plt.xlabel('Distance from Mean')
plt.ylabel('Count')
plt.show();
```


*Standardising data*
Standardisaton, also known as Z-Score normalisation, is when we center our dataa, then divide it by the standard deviation. This makes our data have the mean at 0 and a standard deviation of 1, allowing all of our features to be on the same scale. Very useful before:
- Principal component analysis
- Any clustering or distance based algorithm (KMeans or DBScan)
- KNN
- Performing regularisation methods such as lasso and ridge

The formula is z = (value-mean)/SD

```py
distance = coffee['nearest_starbucks']
 
#find the mean of our feature
distance_mean = np.mean(distance)
 
#find the standard deviation of our feature
distance_std_dev = np.std(distance)
 
#this will take each data point in distance subtract the mean, then divide by the standard deviation
distance_standardized = (distance - distance_mean) / distance_std_dev
```

_Standardisation using Sklearn_
```py
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

#The scaler needs the data into a column so we have to reshape
reshaped_data = np.array(data).reshape(-1, 1) #the -1 is to say as many rows as needed

#Scale the data
scaled_data = scaler.fit_transform(reshaped_data)

print(np.mean(distance_scaler))
#output = -9.464196275493137e-17
print(np.std(distance_scaler))
#output = 0.9999999999999997
```




*Min-Max normalisation*
The process consists of finding the minimum and maximum data point and set those to 0 and 1 respectively. The rest of the data points will transform to a number between 0 and 1, depending on its distance between the minimum and the maximum. This is done by taking the data point, subtracting it from the minimum, then dividing by the value of our maximus minus minimum.Ex
xnorm = (x - xmin)/(xmax - xmin)

This des not work well with data that has extreme outliers
```py
distance = coffee['nearest_starbucks']
 
#find the min value in our feature
distance_min = np.min(distance)
 
#find the max value in our feature
distance_max = np.max(distance)
 
#normalize our feature by following the formula
distance_normalized = (distance - distance_min) / (distance_max - distance_min)
```


_Min-Max with Sklearn_
```py
from sklearn.preprocessing import MinMaxScaler
mmscaler = MinMaxScaler()

#get our distance feature
distance = coffee['nearest_starbucks']
 
#reshape our array to prepare it for the mmscaler
reshaped_distance = np.array(distance).reshape(-1,1)
 
#.fit_transform our reshaped data
distance_norm = mmscaler.fit_transform(reshaped_distance)
 
#see unique values
print(set(np.unique(distance_norm)))
#output = {0.0, 0.125, 0.25, 0.375, 0.5, 0.625, 0.75, 0.875, 1.0}
```



*Binning*
Binning our data is the process of taking numerical or categorical data and breaking it up into groups. (Basically categorising your numerical data). Ex for a DF with distance in km we can do <1 >1|<5 >5

```py
#Set the boundaries
bins = [0, 1, 3, 5, 8.1]
#!! The 8.1 is bc that's the exact cutoff, we want to have the 8km together with the 5km


#'cut' the df in the defined values
coffee['binned_distance'] = pd.cut(coffee['nearest_starbucks'], bins, right = False)
 
print(coffee[['binned_distance', 'nearest_starbucks']].head(3))
 
#output
#  binned_distance  nearest_starbucks
#0      [5.0, 8.1)                  8 -> in the distance the [ includes the value while ) excludes it
#1      [5.0, 8.1)                  8 -> eg 5 <= x < 8.1 (that's why 8.1 and not 8)
#2      [5.0, 8.1)                  8



# Plot the bar graph of binned distances
coffee['binned_distance'].value_counts().plot(kind='bar')
 
# Label the bar graph 
plt.title('Starbucks Distance Distribution')
plt.xlabel('Distance')
plt.ylabel('Count') 
 
# Show the bar graph 
plt.show()
```



*Natural Log transformation*
This transformation wors well for right-skewed data (majority of data points are on the left) and with large outliers. This also changes the scale so our data points will drastically reduce the range of their values. This will require some extra interpretation. Also it is not the best answer for skewed data, especially if •you have values <0 (log of negative number is undefined) •you have left-skewed data (might need a square or cube tranformation) •you have non-parametric data.

```py
import numpy as np
 
#perform the log transformation
log_car = np.log(cars['odometer'])
 
#graph our transformation
plt.hist(log_car, bins = 200, color = 'g')
 
#rotate the x labels so we can read it easily
plt.xticks(rotation = 45)
 
#provide a title
plt.title('Logarithm Transformation')
plt.show()
```






# Encoding categorical variables
Categorical data is data that has more than one category. It can be nominal or ordinal (with inherent hierarchy)

*Ordinal encoding*
We can do this with two approaches. 
We create a dictionary where every label is the key and the numeric number is the value, then we map each label from the condition column to the numeric value in a new column.
```py
# create dictionary of label:values in order
rating_dict = {'Excellent':5, 'New':4, 'Like New':3, 'Good':2, 'Fair':1}
 
#create a new column 
cars['condition_rating'] = cars['condition'].map(rating_dict)
```

The other approach involves sklearn, with a similar approach.
```py
# using scikit-learn
from sklearn.preprocessing import OrdinalEncoder
 
# create encoder and set category order
encoder = OrdinalEncoder(categories=[['Excellent', 'New', 'Like New', 'Good', 'Fair']])
 
# reshape our feature
condition_reshaped = cars['condition'].values.reshape(-1,1)
 
# create new variable with assigned numbers
cars['condition_rating'] = encoder.fit_transform(condition_reshaped)
```



*Label encoding*
Ex we have our colour feature with a lot of different labels, and we need to encode the top 5 colours to numbers.
```py
# convert feature to category type
cars['color'] = cars['color'].astype('category')
 
# save new version of category codes
cars['color'] = cars['color'].cat.codes
 
# print to see transformation
print(cars['color'].value_counts()[:5])
# #OUTPUT
# 2     2015
# 18    1931
# 8     1506
# 15    1503
# 3      869
```
This could cause some inherent 'weight' issue, making different colours more or less valuable, for this we use one-hot encoding, or sklearn

```py
from sklearn.preprocessing import LabelEncoder
 
# create encoder
encoder = LabelEncoder()
 
# create new variable with assigned numbers
cars['color'] = encoder.fit_transform(cars['color'])
```





*One hot encoding*
One-hot encoding is when we create a dummy variable for each value of our categorical feature. A dummy variable is a numeric variable with only values 1 and 0. 
(A downside is that it'll add many many columns (one for each unique value))
```py
import pandas as pd
# use pandas .get_dummies method to create one new column for each color
ohe = pd.get_dummies(cars['color'])
 
# join the new columns back onto our cars dataframe
cars = cars.join(ohe)
```





*Binary encoding*
Strong alternative to one-hot encoding. Binary encoding will find the number of unique categories and then convert each category to its binary representation. (if we have 19 categories we only need 5 new columns)

```py
from category_encoders import BinaryEncoder
 
#this will create a new data frame with the color column removed and replaced with our 5 new binary feature columns
colors = BinaryEncoder(cols = ['color'], drop_invariant = True).fit_transform(cars)
```


*Hashing*
Encoding technique similar to binary and one hot encoding. Its advantageous as it reduces dimensionality but some categories might be mapped to the same values (_called collision_) causing issues in the analyisis.

```py
from category_encoders import HashingEncoder
 
# instantiate our encoder
encoder = HashingEncoder(cols='color', n_components=5)
 
# do a fit transform on our color column and set to a new variable
hash_results = encoder.fit_transform(cars['color'])
```





*Target encoding*
Target encoding (mean encoding) is a Bayesian encoder used to transform categorical features into hashed numerical values. This is used for data sets being prepared for regression-based supervised learning. The numerical values of each category is replaced with a blend of the posterior probability of the target given a particular categorical value and the prior probability of the target over all the training data.
```py
from category_encoders import TargetEncoder
 
# instantiate our encoder
encoder = TargetEncoder(cols = 'color')
 
# set the results of our fit_transform to a variable 
# the output will be its own pandas series
encoder_results = encoder.fit_transform(cars['color'], cars['sellingprice'])
 
print(encoder_results.head())
#   color
# 0    11761.881473
# 1    18007.276995
# 2    8458.251232
# 3    14769.292595
# 4    12691.099747
```




*Encoding date-time variables*

```py
print(cars['saledate'].dtypes)
# # OUTPUT
# dtype('O')
 
cars['saledate'] = pd.to_datetime(cars['saledate'])
# #OUTPUT
# datetime64[ns, tzlocal()]

#!!!! NOW we can access features

# create new variable for month
cars['month'] = cars['saledate'].dt.month
 
# create new variable for day of the week
cars['dayofweek'] = cars['saledate'].dt.day
 
# create new variable for difference between cars model year and year sold
cars['yearbuild_sold'] = cars['saledate'].dt.year - cars['year']
```
















