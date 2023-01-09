# Supervised learning
Supervised learning is at the hearth of ML. It typically depends on human-generated training data that leveragtes computational approaches to finding patterns and making generalisations.
Here is covered:
- Simple linear regression
- Multiple linear regression
- Logistic regression
- The K-nearest neighbors algorithm
- Decision trees algorithms
- Evaluation metrics to assess performances.

The two types of supervised learning algorithms are:
- Regression
	+ Regression is used to predict continuous outputs
- Classification
	+ Classification is used to predict discrete labels. The outputs come under a finite amount. This is split in 
		* Binary classification
		* Multi-class classification
			- It determines one outcome from the possible choices
		* Multi-label classification
			- It attributes >=1 'label' to each piece of data (used for segmentation, etc)
			- ex. an image with both a cat and a bird will have 2 labels (cat bird)





## Linear regression
Linear regression tries to find a line that best fits a set of data. 
Linear equation: y = mx + b
The scope of linear regression is to find m and b and plot our y to understand the expected x.
When we calculate the best fitting line, we calculate the _loss_ that each possible line produces cumulatively, picking the one with the least _loss_. We do this in a fashion that is called *gradient descent* eg we move in the direction that decreases our loss the most. The *gradient* refers to the slope of a curve at any point. The formula is 
`-2/N ∑(yi - (mxi + b))`
- N is the number of points in our dataset
- m is the current gradient guess
- b is the current interceipt guess

Broken down:
- We find the sum of `y_value - (m*x_value + b)` for all the y_value and x_value that we have
- We then multiply the sum by a factor of -2/N

Coded version
```py
import matplotlib.pyplot as plt

def get_gradient_at_b(x, y, b, m):
  N = len(x)
  diff = 0
  for i in range(N):
    x_val = x[i]
    y_val = y[i]
    diff += (y_val - ((m * x_val) + b))
  b_gradient = -(2/N) * diff  
  return b_gradient

def get_gradient_at_m(x, y, b, m):
  N = len(x)
  diff = 0
  for i in range(N):
      x_val = x[i]
      y_val = y[i]
      diff += x_val * (y_val - ((m * x_val) + b))
  m_gradient = -(2/N) * diff  
  return m_gradient

def step_gradient(b_current, m_current, x, y, learning_rate):
    b_gradient = get_gradient_at_b(x, y, b_current, m_current)
    m_gradient = get_gradient_at_m(x, y, b_current, m_current)
    b = b_current - (learning_rate * b_gradient)
    m = m_current - (learning_rate * m_gradient)
    return [b, m]
  
def gradient_descent(x, y, learning_rate, num_iterations):
  b = 0
  m = 0
  for i in range(num_iterations):
    b, m = step_gradient(b, m, x, y, learning_rate)

  return [b, m]


months = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
revenue = [52, 74, 79, 95, 115, 110, 129, 126, 147, 146, 156, 184]

b, m = gradient_descent(months, revenue, 0.01, 1000)

y = [m*x + b for x in months]

plt.plot(months, revenue, "o")
plt.plot(months, y)

plt.show()
```


SciKit version
```py
from sklearn.linear_model import LinearRegression

line_fitter = LinearRegression()
line_fitter.fit(X, y)

# Slope
line_fitter.coef_

# Interceipt
line_fitter.interceipt_

# Pass x value to predict y value
y = line_fitter.predict(X)
```









## Multiple Linear regression
Multiple linear regression uses two or more independent variables to predict the values of the dependent variable. It is based on the equation:
`y = b + m1x1 + m2x2 + ... + mnxn`

*Training Set vs. Test Set*
As with most machine learning algorithms we have to split our dataset into:
- Training set
	+ Data used to fit the model (usually 80%)
- Test set
	+ Data partitioned away at the very start of the experiment to provide unbiased evaluation (usually 20%)

```py
from sklearn.model_selection import train_test_split
 
x_train, x_test, y_train, y_test = train_test_split(x, y, train_size=0.8, test_size=0.2)

#train size is proportion of training, test size is for test
# random_state is an optional seed used by the random number generator

```


*Build the model*
Now that we have the training set, we can use it to build the regression model:
```py
from sklearn.linear_model import LinearRegression
mlr = LinearRegression()
 
mlr.fit(x_train, y_train) 
# finds the coefficients and the intercept value


#The model takes values calculated by `.fit()` and the `x` values, plugs them into the multiple linear regression equation, and calculates the predicted y values. 
y_predicted = mlr.predict(x_test)


# Again you can calculate slopes and interceipts, the slopes will give insight into which variables 'weight' more
# Slope
print(mlr.coef_)

#!!! This returns a numpy array of values in order of input

# Interceipt
print(mlr.interceipt_)
```


*Visualise the results*
This type of data can be visualised with a 2D scatterplot
```py
# Create a scatter plot
plt.scatter(x, y, alpha=0.4)
 
# Create x-axis label and y-axis label
plt.xlabel("the x-axis label")
plt.ylabel("the y-axis label")
 
# Create a title
plt.title("title!")
 
# Show the plot
plt.show()
```



*Evaluating the model's accuracy*
One technique used is _Residual Analysis_. This calculates the difference between the actual value y and the predicted value ^y, which is the residual e. `e = y - ^y`

In sklearn, there is a method that returns the *coefficient of determination R^2* of the prediction. 
Defined as `1 - u/v` where:
- u: residual sum of squares `((y - y_predict) ** 2).sum()`
- v: total sum of squares (TSS) `((y - y.mean()) ** 2).sum()`
	+ TSS also tells you how much variation there is in the y variable.

*R^2* is the percentage variation in y explained by all the x variables together. (ex our R^2 if 0.72, that means that our model explains 72% variation in y, 0.70 is what ppl aim at)

_In python_
```py
#Mean squared error regression loss
#Returns R^2
print(mlr.score(x_train, y_train))
```




### SGDRegressor in short
Stochastic gradient descent (or SGD for short). SGD is very similar to gradient descent, but instead of using the actual gradient it uses an approximation of the gradient that is more efficient to compute. This model is also sophisticated enough to adjust the learning rate as the SGD algorithm iterates, so in many cases you won’t have to worry about setting the learning rate.

```py
from sklearn.datasets import load_diabetes
from sklearn.linear_model import SGDRegressor
 
# Import the data set
X, y = load_diabetes(return_X_y=True)
 
# Create the SGD linear regression model
# max_iter is the maximum number of iterations of SGD to try before halting
sgd = SGDRegressor(max_iter = 10000)
 
# Fit the model to the data
sgd.fit(X, y)
 
# Print the coefficients of the model
print(sgd.coef_)
 
# Print R^2
print(sgd.score(X, y))
```








## Logistic regression
Logistic regression is a supervised machine learning algorithm that predictes the probability ,ranging from 0 to 1 of a datapoint belonging to a specific category, or class. These are often used to classify observations. Ex we can use a model to predict whether the email incoming is spam or not.

Logistic regression is similar to linear regression, other than the fact that uses a logarithmic function which acquires an s shape in the graph (sigmoid function)(logit link):
`ln(y/1-y)= b0 + b1x1 + b2x2 + ... + bnxn` -> y can be changed with p which stands for _probability_

*Log odds*
Given the ln in the equation, the result is called log odds, from this we can get the odds as:
odds = p/1-p = P(event occurring)/P(event not occurring)
The odds transform the probability to a continuous value that goes from -∞ to +∞

_With sklearn_
```py
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()

model.fit(features, labels) # categorical, numerical ex purchase, min_on_site

model.coef_ #is a vector of the coefficients of each feature
model.intercept_ #is the intercept

#Get the odds
log_odds = model.intercept_ + model.coef_ * labels 
print(log_odds)

#Get the probability 
probability = np.exp(log_odds)/(1+ np.exp(log_odds))

```
*Coefficient interpretation*
- Large positive: a one unit increase is associated with a _large_ increase in the log odds (therefore probability) of a datapoint belonging to the positive class (outcome group 1)
- Large negative: same as before but decrease
- Coefficient of 0: the feature is not associated with the outcome

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! Sklearn logistic regression _requires the features to be standardised because regularisation is implemented by default_.


*Predictions with sklearn*
```py
#Predicts expected outcome
print(model.predict(features))

#Predicted probabilities for each feature in one of the two inputed groups
print(model.predict_proba(features)[:,1])


```

*Classification thresholding*
Usually in sklearn the default threshold is 0.5, if the predicted probability of an observation belonging to the positive class is >=0.5, the datapoint is assigned a positive class. We should lower this for more sensitivity, but also an increased rate of false positives.




*Confusion matrix*
A confusion matrix is used to show the number of true/false positives, and true/false negatives:
```py

y_true = [0, 0, 1, 1, 1, 0, 0, 1, 0, 1]
y_pred = [0, 1, 1, 0, 1, 0, 1, 1, 0, 1]

from sklearn.metrics import confusion_matrix
print(confusion_matrix(y_true, y_pred))

#array([[3, 2],
#       [1, 4]])
```
eg.
3 true negatives, 1 false negative
4 true positives, 2 false positive

*Accuracy, Recall, Precision, F1 Score*
Once we have a confusion matrix, there are a few different statistics we can use to summarize the four values in the matrix. These include accuracy, precision, recall, and F1 score. For all of these metrics, a value closer to 1 is better and closer to 0 is worse.
(T = true, F = false, P = positive, N = negative)
- Accuracy = (TP + TN)/(TP + FP + TN + FN)
- Precision = TP/(TP + FP)
- Recall = TP/(TP + FN)
- F1 score: weighted average of precision and recall

```py
# accuracy:
from sklearn.metrics import accuracy_score
print(accuracy_score(y_true, y_pred))
# output: 0.7
 
# precision:
from sklearn.metrics import precision_score
print(precision_score(y_true, y_pred))
# output: 0.67
 
# recall: 
from sklearn.metrics import recall_score
print(recall_score(y_true, y_pred))
# output: 0.8
 
# F1 score
from sklearn.metrics import f1_score
print(f1_score(y_true, y_pred))
# output: 0.73
```







