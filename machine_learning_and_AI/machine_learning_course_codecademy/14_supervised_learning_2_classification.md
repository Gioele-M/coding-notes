# Supervised learning - Classification algorithms


## K-Nearest neighbors classifier
KNN is a classification algorithm that groups data points with similar attributes into similar categories.

*Point distance in 2D*
distance = sqrt((x1 - x2)^2 + (y1 - y2)^2)

*Point distance in 3D*
The generalised formula for distance between points is simply
distance = sqrt((x1 - x2)^2 + (y1 - y2)^2 + ... + (n1 - n2)^2)


KNN implements 3 main steps:
- Normalisation of data
- Finding the `k` nearest neighbors
- Classify the points based on those neighbors


*Normalisation*
Normalisation is essential so to give the same weight to each datapoint. Ex we're comparing years movies came out and budget, without normalisation 70 years would have been put on the 'scale' as if it was 70$ difference in budget.


*Choosing K*
The validation accuracy changes as `k` changes. There can be a couple issues:
- Overfitting: this happens when k is very small. This occurs when you rely too heavily on your training data, in the case of KKN is when you don't consider enough neighbors.
- Underfitting: this happens when k is too large. This occurs when our model is too generic and doesn't pay attention to details in the training set



_In SkLearn_
```py
from sklearn.neighbors import KNeighborsClassifier

#Initialise the classifier with the desired number of k
classifier = KNeighborsClassifier(n_neighbors = 3)


#!!!!!!!!!!!!!!!!!!!!!!!Notice how this data is alrady normalised
#Train the classifier with the list of points, and labels associated with those
training_points = [
  [0.5, 0.2, 0.1],
  [0.9, 0.7, 0.3],
  [0.4, 0.5, 0.7]
]
 
training_labels = [0, 1, 1]
classifier.fit(training_points, training_labels)


# Finally we can use the predict() method to guess the category of our unknown points
unknown_points = [
  [0.2, 0.1, 0.7],
  [0.4, 0.7, 0.6],
  [0.5, 0.8, 0.1]
]
 
guesses = classifier.predict(unknown_points)
```



### K Nearest neighbor regressor
KNN can classify data based on their variables, but it can also estimate the a rating when trained with appropriate labels. Usually this is done by taking the _weighted average_ of the values that populate the category.

_In SkLearn_
```py
from sklearn.neighbors import KNeighborsRegressor

#Define the regressor, the weight keyword rquals 'uniform' when the average is used as metric, otherwise 'distance' if the weighted average is used

classifier = KNeighborsRegressor(n_neighbors = 3, weights = "distance")


# Train the model as before
training_points = [
  [0.5, 0.2, 0.1],
  [0.9, 0.7, 0.3],
  [0.4, 0.5, 0.7]
]
 
training_labels = [5.0, 6.8, 9.0]
classifier.fit(training_points, training_labels)


# Use predict to produce the preditictions
unknown_points = [
  [0.2, 0.1, 0.7],
  [0.4, 0.7, 0.6],
  [0.5, 0.8, 0.1]
]
 
guesses = classifier.predict(unknown_points)
```
















-------
## Decision trees
Decision trees are ML models that try to find patterns in the features of data points.
It basically creates a algorithm where each node is a choice and depending on the answer the data point has for the algotithm it gets labelled (eg is the value true/>1.5/etc)
It is a cyclical process of splitting groups in subsets that respect certain conditions, until we reach the leaf of the tree which is our label.

_SkLearn example_
The dataset used has 6 categorical features in the DF.
The question is: when considering buying a car, what factors go into making a decision?

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


#Import models from scikit learn module:
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn import tree

#Loading the dataset
df = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data', names=['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'accep'])

## Take a look at the dataset
print(df.head())

## Setting the target and predictor variables
df['accep'] = ~(df['accep']=='unacc') #1 is acceptable, 0 if not acceptable
#This ~ means inverse

#Gets array of columns up to accept as categorical values 
X = pd.get_dummies(df.iloc[:,0:6])
#Gets the accep values for the labels
y = df['accep']


## Performing the train-test split
x_train, x_test, y_train, y_test = train_test_split(X,y, random_state=0, test_size=0.2)


## Fitting the decision tree classifier
dt = DecisionTreeClassifier(max_depth=3, ccp_alpha=0.01,criterion='gini', min_impurity_decrease=0.02)
dt.fit(x_train, y_train)


# Predict on test data
y_pred = dtree.predict(x_test)


#Check for accuracy
print(f'Test set accuracy: {dtree.score(x_test, y_test)}') # or accuracy_score(y_test, y_pred)

#Check for tree depth
dtree1_depth = dtree1.get_depth()


## Plotting the Tree
plt.figure(figsize=(20,12))
tree.plot_tree(dt, feature_names = x_train.columns, max_depth=5, class_names = ['unacc', 'acc'], label='all', filled=True)
plt.tight_layout()
plt.show()
```

In `DecisionTreeClassifier()`:




*Tree inteerpretation*
The root of the tree is at the top. This has the number of samples and numbers in each class that was used to build the tree. The splits occurs as True to the left, False to the right. The final component of a branch is a lieaf node, any decision making here ends up in the majority class.
*The gini value* calculates the 'Gini impurity' of a set of datapoints. For two classes 1 and 2, with probabilities p_1 and p_2, the gini impurity is:
`1 - (p1^2 + p2^2) = 1 - (p1^2 + (1 - p1)^2)`

The goal of the algorithm is to separate the classes the best possible (ie minimize impurity). If p_1 is 0 or 1, the Giny impurity is 0, meaning that the classes are perfectly pure, it is at is peak when p_1 = 0.5.
General definition for c classes: `1 - ∑(c) pi^2`


*Information gain*
The information gain is a metrics that help figure out which features to split on the data. It measures the difference in the impurity of the data before and after the split (`gini_coef_after - gini_coef_before`). If the result is 0 the split was useless.



*ADVANTAGES AND DISADVANTAGES*
*Disadvantages*
- The trees arent always globally optimal. Meaninng that there might be a better tree that produces better results. This is because our strategy is _greedy_. We assume that the best way to create a tree is gain *right now* and split on that feature without considering ramifications.
- Trees are prone to overfit the data, so it's too dependent on the training data.





### Decision trees for Classification and Regression
Decision trees can also be used for regression tasks (ex predict the number of spam mails per day). The algorithm works similarly with modification only to the splitting criteria and how the final output is computed.
*MSE* is the mean squared error which is the default parameter investigated in regression

*Example*
```py
# Similar to normal but the y must be real-valued and the model must be DecisionTreeRegressor

x_train, x_test, y_train, y_test = train_test_split(X,y, random_state=0, test_size=0.2)
dt = DecisionTreeRegressor(max_depth=3, ccp_alpha=0.001)
dt.fit(x_train, y_train)
y_pred = dt.predict(x_test)
print(dt.score(x_test, y_test))

#Check tree
plt.figure(figsize=(10,10))
tree.plot_tree(dt, feature_names = x_train.columns,  
               max_depth=2, filled=True);


#Code Challenge
def mse(data):
    """Calculate the MSE of a data set
    """
    return np.mean((data - data.mean())**2)
 
def mse_gain(left, right, current_mse):
    """Information Gain (MSE) associated with creating a node/split data based on MSE.
    Input: left, right are data in left branch, right banch, respectively
    current_impurity is the data impurity before splitting into left, right branches
    """
    # weight for gini score of the left branch
    w = float(len(left)) / (len(left) + len(right))
    return current_mse - w * mse(left) - (1 - w) * mse(right)
 
m = None
print(f'MSE at root: {round(m,3)}')
 
mse_gain_list = []
for i in x_train.cgpa.unique():
    left = y_train[x_train.cgpa<=i]
    right = y_train[x_train.cgpa>i]
    mse_gain_list.append([i, mse_gain(left, right, m)])
 
mse_table = pd.DataFrame(mse_gain_list,columns=['split_value', 'info_gain']).sort_values('info_gain',ascending=False)
print(mse_table.head(10))
 
print(f'Split with highest information gain is: {None}')
 
plt.plot(mse_table['split_value'], mse_table['info_gain'],'o')
plt.plot(mse_table['split_value'].iloc[0], mse_table['info_gain'].iloc[0],'r*')
plt.xlabel('cgpa split value')
plt.ylabel('info gain')

```






















# Normalisation

## Min-Max normalisation
For every feature the minimum value gets tranformed to 0, the maximum value to 1. All the others are (value - min)/(max-min).
The significant downside of this technique is that it doesn't handle outliers very well.

## Z score normalisation
Z-Score normalisation is a strategy of normalising data that avoids the outlier issue using: 
(value - mean)/SD. With this if the value is on the mean gets normalised to 0, any other value goes positive or negative depending on where they lie (the range of values is determined by the standard deviation).



# Training, validation and test datasets
- Training set
	+ Data that the model will learn hot to make predictions from. 
- Validation set
	+ Data that will be used during the training phase to evaluate the interim performance of the model, guide the tuning of hyper-parameters, and assist in other model improvement capacities. (Based on metrics such as accuracy, recall, precision and F1-score)
- Test set
	+ Data that will determine the performance of our final model so that we can estimate how our model will do in the real world.


*Evaluating the model*
During the model fitting, both the features (X), and the true labels (y) are used to learn. Usually the data is split into 70% training, and 15% into each of the validation adn test. In case this is impossible (for lack of data) we can do the *N-Fold Cross-Validation*: basically we split the data in rotation to get every time a different chunk of test data, then we average the results.




# Evaluation metrics for classification


### Confusion matrix
T = True, F = False, P = Positive, N = Negative
All of our predictions fall under TN TP FN FP. The confusion matrix helps us visualise these values.

.		Predicted -	 Predicted +
Actual -	TN			FP
Actual +	FN			TP

```py
from sklearn.metrics import confusion_matrix
conf_matrix = confusion_matrix(actual, predicted)
```



### Accuracy
Accuracy is calculated finding the total number of correctly classified predictions and dividing by the total number of predictions:
*Accuracy = (TP + TN) / (TP + TN + FP + FN)*


### Recall
Recall is a metric that is more accurate then accuracy when considering that the number of true positive could be very small. It is defined as *TP/(TP+FN)*


### Precision
Precision is the ratio of correct positive to all positive classification. 
*Precision = TP/(TP+FP)*


### F1-Score
Useful to consider both the precision and recall when attempting to describe the effectiveness of a model, determining their harmonic mean (this is because we want it to have low value if one of the two metrics is low)
*F1-score = (2 * precision * recall)/(precision + recall)*

Ex: If recall is 1 but precision 0.02, the F1 score would be extremely low.


_In SkLearn_
```py
from sklearn.metrics import accuracy_score, recall_score, precision_score, f1_score

actual = [1, 0, 0, 1, 1, 1, 0, 1, 1, 1]
predicted = [0, 1, 1, 1, 1, 0, 1, 0, 1, 0]

print(accuracy_score(actual, predicted))

print(recall_score(actual, predicted))

print(precision_score(actual, predicted))

print(f1_score(actual,predicted))
```





















