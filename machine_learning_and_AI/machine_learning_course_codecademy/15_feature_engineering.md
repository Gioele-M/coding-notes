# Feature selection methods
The goal is to improve the model's accuracy and efficiency byy eliminating extraneous variables that do not contribute to any useful information:
- Filter methods
- Wrapper methods
- Embedded methods


#### Filter methods
Simplest type of feature selection method. They work by filtering features prior to model building based on some criteria.
*Advantages*
- Computationally inexpensive
- Can work for any type of ML
*Disadvantages*
- Makes multivariate relationships more difficult
- Not tailored toward specific models
*Examples*
- Variance thresholds
- Correlation
- Mutual information

#### Wrapper methods
Involve fitting a model and evaluating its performance for a particular subset of features. The work by using a search algorithm to find which combination of features can optimise the performance of a given method.
*Advantages*
- Can determine the optimal set of features for the model
- Can better account multivariate relatioships bc what is evaluated is the model performance
*Disadvantages*
- Computationally expensive
*Examples*
- Forward/backward/bidirectional feature selection
- Recursive feature elimination


#### Embedded methods
Involve building and evaluating models for different feature subsets. They differ in their selection process happens at the same time as their model fitting step.
*Advantages*
- Can optimise feature selection for multivatiate relationships
- Generally less computationally expensive
*Examples*
- Regulatisation (lasso/ridge regression)
- Tree-based feature importance






--------






## Filter methods
Filter methods work by selecting features based on some criteria prior tu building the model. They're computationally inexpensive and an efficient initial step for narrowing down the pool of features to the most relevant ones. 

*Example dataset with students data*
```py
import pandas as pd
 
df = pd.DataFrame(data={
    'edu_goal': ['bachelors', 'bachelors', 'bachelors', 'masters', 'masters', 'masters', 'masters', 'phd', 'phd', 'phd'],
    'hours_study': [1, 2, 3, 3, 3, 4, 3, 4, 5, 5],
    'hours_TV': [4, 3, 4, 3, 2, 3, 2, 2, 1, 1],
    'hours_sleep': [10, 10, 8, 8, 6, 6, 8, 8, 10, 10],
    'height_cm': [155, 151, 160, 160, 156, 150, 164, 151, 158, 152],
    'grade_level': [8, 8, 8, 8, 8, 8, 8, 8, 8, 8],
    'exam_score': [71, 72, 78, 79, 85, 86, 92, 93, 99, 100]
})

# 10 x 6 features matrix
X = df.drop(columns=['exam_score'])
 
# 10 x 1 target vector
y = df['exam_score']
```


#### *Variance threshold*
One of the most basic filter methods, it consists in removing any feature with little to no variation in their values (bc they don't contribute as much to the model). This method only works on quantitative features (can do for categorical too but more discretion!!).

```py
from sklearn.feature_selection import VarianceThreshold
 
selector = VarianceThreshold(threshold=0)  # 0 is default, can be changed to higher values

#X_num is bc we only fed numerical variables!
print(selector.fit_transform(X_num))

# Specify `indices=True` to get indices of selected features
print(selector.get_support(indices=True))


# Use indices to get the corresponding column names of selected features
num_cols = list(X_num.columns[selector.get_support(indices=True)])

# Subset `X_num` to retain only selected features
X_num = X_num[num_cols]
```





#### *Pearson's correlation*
There are 2 main ways of using correlation for reature selection: correlation between features, and correlation between a feature and a target variable.

*Correlation between features*
When two features are highly correlated with one another, then keeping just one to be used in the model will be enough, otherwise it would provide duplicate information. We can visualise this in a heatmap:
```py
import matplotlib.pyplot as plt
import seaborn as sns
 
corr_matrix = X_num.corr(method='pearson')  # 'pearson' is default
 
sns.heatmap(corr_matrix, annot=True, cmap='RdBu_r')
plt.show()
```
We define high correlation as > 0.7 or < -0.7
*Print* variables with high correlation:
```py
# Loop over bottom diagonal of correlation matrix
for i in range(len(corr_matrix.columns)):
    for j in range(i):
 
        # Print variables with high correlation
        if abs(corr_matrix.iloc[i, j]) > 0.7:
            print(corr_matrix.columns[i], corr_matrix.columns[j], corr_matrix.iloc[i, j])

```
To decide which variable to remove, we have to look at the correlation with the target variable, then remove the last one that is less associated with the target.




*Correlation between feature and target*
Basically we remove any variable with correlation < 0.3 or > -0.3 as it might not be very predictive

```py
# Obtain correlation between target variable and rest of the features

# First remove categorical variables
X_y = X_num.copy()
X_y['exam_score'] = y
 

# We generate the correlation matrix and isolate the column corresponding to the target variable to see the correlation
corr_matrix = X_y.corr()
 
# Isolate the column corresponding to `exam_score`
corr_target = corr_matrix[['exam_score']].drop(labels=['exam_score'])
 
sns.heatmap(corr_target, annot=True, fmt='.3', cmap='RdBu_r')
plt.show()


X = X.drop(columns=['hours_TV'])
```



*F-Regression*
F-statistic is larger (and p-value smaller) for predictors that are more highly correlated with the target variable. It returns the f-statistic and the p-value in order. The stronger the correlation, the higher the f-statistic.

```py
from sklearn.feature_selection import f_regression
 
print(f_regression(X_num, y))
```







#### Mutual information
Mutual information is a measure of dependence between two variables and can be used to gauge how much a feature contributes to the preditiction of the target variable. Similat to the Pearson's correlation, but not limited to linear association!! It also works on discrete features, although they have to be numerically encoded first!!.

```py
#!!!! ENCODE DISCRETE COLUMN
from sklearn.preprocessing import LabelEncoder
 
le = LabelEncoder()
 
# Create copy of `X` for encoded version
X_enc = X.copy()
X_enc['edu_goal'] = le.fit_transform(X['edu_goal'])
 


# Compute mutual information between each feature and our y of interest
from sklearn.feature_selection import mutual_info_regression

# Point out the column of discrete featureS!!!!
print(mutual_info_regression(X_enc, y, random_state=68, discrete_features=[0]))
#|!!! if it was a discrete target variable it would have been
# mutual_info_classif()
```
The result is a non-negative number, the higher the value, the more predictive power it has.


*Now* we can select the best features using SelectKBest. We have to specify the scoring function to use and how many top features to select. Partial is used to add additional arguments.

```py
from sklearn.feature_selection import SelectKBest
from functools import partial
 
score_func = partial(mutual_info_regression, discrete_features=[0], random_state=68)
 
# Select top 3 features with the most mutual information
selection = SelectKBest(score_func=score_func, k=3)
 
print(selection.fit_transform(X_enc, y))

#Get it as DF
X = X[X.columns[selection.get_support(indices=True)]]
 
print(X)
```






--------------









## Wrapper methods
A wrapper method for feature selection is an algorithm that selects features by evaluating the performance of a machine learning model on different subsets of features. These algorithms add r remove features one at a time based on how useful those features are to the model.
Their main advantage over the filter methods is that wrapper methods evaluate features based on their performance with a specific model. Wrapper methods take also in account relationships between features. Presented here:
- Sequential forward selection
- Sequential backward selection
- Sequential forward floating selection
- Sequential backward floating selection
- Recursive feature elimination


#### Preparation
Before we use a wrapper model, we need to specify a machine learning model. In this example is logistic regression.

```py
import pandas as pd
from sklearn.linear_model import LogisticRegression

# Load the data
health = pd.read_csv("dataR2.csv")
# Split independent and dependent variables
X = health.iloc[:,:-1]
y = health.iloc[:,-1]

# Logistic regression model
lr = LogisticRegression(max_iter=1000)

# Fit the model
lr.fit(X,y)

# Print the accuracy of the model
print(lr.score(X, y))

#-> 0.8017241379310345
```
Now that there is the machine learning model, we can use a wrapper method to choose a smaller feature subset.


#### Sequential forward selection
Is a wrapper method that builds a feature set by starting with no features and adding one feature at a time. The algorithm will train and test the model adding one feature at a time, the one that performs best is kept. The loop is stopped once the desired number of features is reached. (Greedy algorithm as instead of checking by brute force it gets the feature which is the most optimal at the moment)

_Using mlxtend SFS class to implement squential forward selection_
```py
from mlxtend.feature_selection import SequentialFeatureSelector as SFS

# Set up SFS parameters
sfs = SFS(lr,			#model that you're using (in this case lr)
           k_features=3, # number of features to select
           forward=True,
           floating=False,
           scoring='accuracy', #how the algorithm evaluates based on this, it's ok to leave None as the best one will be adapted
           cv=0)#Allows to do k-fold cross-validation

# Fit SFS to our features X and outcome y   
sfs.fit(X, y)


#Check what metrics were used
print(sfs.subset_)

{1: {'feature_idx': (7,),
  'cv_scores': array([0.93852459]),
  'avg_score': 0.9385245901639344,
  'feature_names': ('FWI',)},
 2: {'feature_idx': (4, 7),
  'cv_scores': array([0.97540984]),
  'avg_score': 0.9754098360655737,
  'feature_names': ('DMC', 'FWI')},
 3: {'feature_idx': (1, 4, 7),
  'cv_scores': array([0.9795082]),
  'avg_score': 0.9795081967213115,
  'feature_names': (' RH', 'DMC', 'FWI')}}

# Dictionary with the number of current features as key, index of current features, avg_score is the accuracy, and a tuple of the column names.

# Print a tuple of feature names after 5 features are added
print(sfs.subsets_[5]['feature_names'])

# Print the accuracy of the model after 5 features are added
print(sfs.subsets_[5]['avg_score'])


# If you want to plot it
from mlxtend.plotting import plot_sequential_feature_selection as plot_sfs
import matplotlib.pyplot as plt
 
# Plot the accuracy of the model as a function of the number of features
plot_sfs(sfs.get_metric_dict())
plt.show()
```



#### Sequential backward selection
This is another wrapper method for feature selection, similar to the frward one. The difference is that this one starts with all of the available features and removes one featue at a time. It runs all subsets with -1 feature, and removes the feature which missing performed the best, and so on. This is the same as before, but requires `forward=False`. The results are also the same, it only has the dictionary backwards as it starts with more elements





#### Sequential Forward and Backward floating
*Sequential forward floating selection* is a variation of sequential forward selection. It starts with zero features and adds one feature at a time, just like sequential forward selection, but after each addition, it checks to see if we can improve performance by removing a feature (careful not to get stuck in a infinite loop).
*Sequential backward floating selection* works similarly. Starting with all available features, it removes one feature at a time. After each feature removal, it will check to see if any feature additions will improve performance.

_In MLXTEND_ we just need to change the parameter `floating=True` and is done, the results will store the best option for each iteration.






#### Recursive Feature elimination
Recursive feature elimination algorithm starts by training a model with all available features, then it ranks each feature according to an importance metric and removes the least important one. This process is repeated till the desired number of features is reached. It differs from sequential backward selection as this one trains the model with the feature in it before removing it, in fact it's much faster.

_In SciKit_
```py
#First standardise the data
from sklearn.preprocessing import StandardScaler
X = StandardScaler().fit_transform(X)



#Now train the model and use RFE using our ml model (in this case lr) and features to select
from sklearn.feature_selection import RFE
 
# Recursive feature elimination
rfe = RFE(lr, n_features_to_select=2)
rfe.fit(X, y)


#Feature evaluation


print(rfe.ranking_) 
#is an array that contains the rank of each feature. 
#[2 5 4 1 3 1]
# In this ranking the features at index 3 and 5 were kept, the others indicate at which step they were removed (the highest number was removed before)


print(rfe.support_)
# Is an array of boolean that is true where the features were kept

# features is a list of feature names
# Get a list of features chosen by rfe
rfe_features = [f for (f, support) in zip(features, rfe.support_) if support]
 
print(rfe_features)



#Accuracy of model
print(rfe.score(X, y))

```







-----------------

# Regularisation
Regulatisation is a statistical technique that minimises overfitting and is executed during the model fitting step. This is also an embedded feature selection method because it is implemented while the parameters of the model are being calculated. (It is rare that is not used).
Regularisation helps model work better with external data.

*Overfitting* is when a model is able to represent a particular set of data points effectively, but cannot fit new data well. The model has >=1 of these attributes:
- It fits the training data well but performs significantly worse on test data
- It typically has more parameters than necessary (eg high model complexity)
- It might be fitting for features that are multi-colinear (features correlated therefore redundant)
- Might be fitting the noise in the data and likely mistaking it for a feature




*The loss function*
The loss function is the function that calculates the loss when using linear regression. The best fit is the one with the highest result for the loss function.
`loss function = 1/n ∑(yi - b0 - b1x1i - b2x21)^2`


*Regularisation* penalises models for overfitting by adding a "_penalty term_" to the loss fuunction (the higher it is, the worse it performs). The two most commonly used ways are _Lasso (L1)_ and _Ridge (L2)_. They both penalise overfitting by controlling how large the coefficients can get in the first place. The penality (or regularisation) term is multiplied by a factor of _alpha_ and added to the old loss function as:
`New loss function = old loss function + alpha * Regularisation term`
The regularisation term can be:
- Lasso: |b1| + |b2| (Manhattan distance)
- Ridge: b1^2 + b2^2 (Euclidean distance)


*Lasso regularisation (“Least Absolute Shrinkage and Selection Operator”)*
With this we're restricting the size of the regularisation term while minimising the old loss function. We're trying to confine our coefficients to a surface around 4 lines b1+b2 = s, b1-b2 = s, b2-b1 = s and -b1-b2 = s (where s is the maximum value our regularisation termm can take). (S is inversely related to alpha). Remember that the goal here is to minimize the original loss function while meeting the regularization constraint. We still want to be as close to the center of the contours (the original loss function minimum) as possible. The point that’s closest to the center of the contours while simultaneously lying within the regularization surface boundary is the white dot!
In models with a high number of features, lasso regularization tends to set a significant fraction of its parameters to zero, thus acting as a feature selection method.
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


df = pd.read_csv("./student_math.csv")
y = df['Final_Grade']
X = df.drop(columns = ['Final_Grade'])

# 1. Train-test split and fitting an l1-regularized regression model
from sklearn.model_selection import train_test_split
from sklearn.linear_model import Lasso
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
lasso = Lasso(alpha = 0.1) #default 1, this determines the amount of features excluded
lasso.fit(X_train, y_train)

l1_pred_train = lasso.predict(X_train)
l1_mse_train = np.mean((l1_pred_train - y_train)**2)
print("Lasso (L1) Training Error: ", l1_mse_train)

# 2. Calculate testing error
l1_pred_test = lasso.predict(X_test)
l1_mse_test = np.mean((l1_pred_test - y_test)**2)
print(l1_mse_test)

# 3. Plotting the Coefficients
predictors = X.columns
coef = pd.Series(lasso.coef_,predictors).sort_values()
plt.figure(figsize = (12,8))
plt.ylim(-1.0,1.0)
coef.plot(kind='bar', title='Regression Coefficients with Lasso (L1) Regularization')
plt.show()
```




*Ridge regularisation*
The key difference of this regularisation is that instead of a diamond-shaped area, we're costraining the coefficients to live within a circle of radius `s`. The general goal is still that we want to minimise the old loss while restricting the values of the coefficients to the boundary of this circle, and we want the new coefficient values to be as close to the unregularised best fit solution.
!! This is not a feature elimination method as L1, the coefficients of L2 get small but never 0! This is useful when we don't want to get rid of features.

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("./student_math.csv")
y = df['Final_Grade']
X = df.drop(columns = ['Final_Grade'])

# 1. Train-test split and fitting an l2-regularized regression model
from sklearn.model_selection import train_test_split
from sklearn.linear_model import Ridge
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
ridge = Ridge(alpha = 100) # default is 1
ridge.fit(X_train, y_train)

#Training error
l2_pred_train = ridge.predict(X_train)
l2_mse_train = np.mean((l2_pred_train - y_train)**2)
print("Ridge (L2) Training Error: ", l2_mse_train)

# 2. Calculate testing error
l2_pred_test = ridge.predict(X_test)
l2_mse_test = np.mean((l2_pred_test - y_test)**2)
print("Ridge (L2) Testing Error: ", l2_mse_test)


# 3. Plotting the Coefficients
predictors = X.columns
coef = pd.Series(ridge.coef_,predictors).sort_values()
plt.figure(figsize = (12,8))
plt.ylim(-1.0,1.0)
coef.plot(kind='bar', title='Regression Coefficients with Lasso (L1) Regularization')
plt.show()
```




*Picking the right alpha*
_Alpha_ is known as a *hyperparameter* in ML. This is chosen prior to fitting the model and used to control the learning process and how much we want to control for overfitting during training.
The larger the alpha, the smaller the size of the coefficient s (which is the size of the area developed by L1 and L2). In  general it is inversely proportional.
- If s is very large (alpha is small): the unregularised loss function will fall easily in this large boundary and thus will make similar choices as without regularisation.
- If s is very small: the regression coefficients become very small and the loss value for the best fit becomes large making the regression over-regularised.
The process of picking the right alpha is called *hyperparameter tuning*





*Bias variance tradeoff*
When adding alpha to our function, we're introductin _bias_ into our problem (the greater the alpha, the smaller s and the the more biased the model). This is the tradeoff between bias and accuracy.


*Differences*

- Regularisation/Penalty term
	+ L1: sum of absolute values of coefficients
	+ L2: sum of squared value of coefficients
- Contraint surface
	+ L1: diamond shaped
	+ L2: circle shaped
- Coefficients after regularisation
	+ L1: All coefficient shrink in their magnitude and some can become 0
	+ L2: All coefficients shrink but never approach 0
- Feature selection method:
	+ L1: Yes, features with magnitude 0
	+ L2: No, but emphasises the relative feature importance








----------

# Implementing L1 and L2 and tuning alpha using GridSearchCV (linear and logistic regression)

*Simple lasso implementation*
```py
import pandas as pd
df = pd.read_csv('student_math.csv')

#Set predictor and outcome and do a train-test-split
y = df['Final_Grade']
X = df.drop(columns = ['Final_Grade'])
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)


# Fit a lasso model
from sklearn.linear_model import Lasso
lasso = Lasso(alpha = 0.05)
lasso.fit(X_train,y_train)

#Check metrics
from sklearn.metrics import mean_squared_error
pred_train = lasso.predict(X_train)
pred_test = lasso.predict(X_test)
training_mse = mean_squared_error(y_train, pred_train)
test_mse = mean_squared_error(y_test, pred_test)
print('Training Error:',  training_mse)
print('Test Error:', test_mse)

#Output
Training Error: 2.8132075838851405
Test Error: 4.474769444129441
```
We notice that the test error is >1.5 times the training error, this means that he model is overfitting the data and can be regularised more.

*In order to lower the errors* we have to try with different alpha values, this can be done automatically with GridSearchCV. This uses a 'k-fold' cross validation method for the optimal hyperparameter. The method consists of splitting the data into train-test 'k' times so that every point is in the test data at least once.
GridSearchCV takes in:
- estimator: the algorithm we're tuning (Lasso() or Ridge())
- param_grid: grid of potential values for the hyperparameter in form of dictionary, in form of keys as parameter inputs, and the values as a list of potential parameters. For this example we are feeding 10^-6 < x < 1 with difference of x10. We do with np.logspace which returns an array of values distantiated logarithmically.
- scoring: metric used to evaluate the performance of the model on the test set (it will maximise this value). The argument for mean squared error, our chosen metric, is neg_mean_squared_error. It’s a negative value because GridSearchCV maximizes the score it’s given.
- cv: specifies the way the cross-validation is performed (it is the k folds the algorithm is going to do)
- return_train_score: boolean specifying whether we want the output of our fit to return the scores on training data every fold (def False)

```py
from sklearn.model_selection import GridSearchCV
model = GridSearchCV(estimator = Lasso(), param_grid = tuned_parameters, scoring = 'neg_mean_squared_error', cv = 5, return_train_score = True)
model.fit(X, y)
```
*Model attributes*
- model.cv_results_ : gives us the details of every model fit corresponding to a particular alpha
- best_estimator_ 
- best_score_
- best_params_

```py
test_scores = model.cv_results_['mean_test_score']
train_scores = model.cv_results_['mean_train_score']
print(model.best_params_, model.best_score_)
```





*Regulatisation with logistic regression classifier*
Logistic regression is already regularised on scikit-learn with L2 and alpha 100, we can change these by tuning the models (C is the inverse of alpha)
```py
model = LogisticRegressionCV( Cs=np.logspace(-3,2, 100), #inverse of alpha
                                  penalty='l2', #type of regularisaiton
                                  scoring='accuracy', cv=5, # number of folds
                                  random_state=42, max_iter=10000)
model.fit(X, y)
print(model.C_, model.scores_[1].mean(axis=0).max())
```








-----------------------




# Feature importance
Features associated with the oucome are considered more 'important'. It is often used for dimensionality reduction, but also for inspection and communication. These can be calculated in 3 main methods:

*Gini impurity*
Gini impurity is related to the extent to which observations are well separated based on the outcome variable at each node of the tree. To estimate feature importance we can calculate the Gini gain.
_Analyse gini gain_
```py
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn import datasets 
 
dataset = datasets.load_breast_cancer()
X = pd.DataFrame(dataset.data, columns=dataset.feature_names)
y = dataset.target
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25)


from sklearn.tree import DecisionTreeClassifier
 
clf = DecisionTreeClassifier(criterion='gini')
 
# Fit the decision tree classifier
clf = clf.fit(X_train, y_train)



# Print the feature importances
feature_importances = clf.feature_importances_



# Visualise in a barchart
import seaborn as sns
 
# Sort the feature importances from greatest to least using the sorted indices
sorted_indices = feature_importances.argsort()[::-1]
sorted_feature_names = data.feature_names[sorted_indices]
sorted_importances = feature_importances[sorted_indices]
 
# Create a bar plot of the feature importances
sns.set(rc={'figure.figsize':(11.7,8.27)})
sns.barplot(sorted_importances, sorted_feature_names)
```
This is a good method but it is biased toward selecting numerical features (over categorical), and does not take into consideration correlation between features.



*Aggregate methods*
Random forests are an ensemble-based machine learning algorithm that utilize many decision trees (each with a subset of features) to predict the outcome variable. Just as we can calculate Gini importance for a single tree, we can calculate average Gini importance across an entire random forest to get a more robust estimate.






*Permutation-based methods*
Another way to test the importance of particular features is to essentially remove them from the model (one at a time) and see how much predictive accuracy suffers. One way to “remove” a feature is to randomly permute the values for that feature, then refit the model. This can be implemented with any machine learning model, including non-tree-based- methods. However, one potential drawback is that it is computationally expensive because it requires us to refit the model many times.




*Coefficients*
When we fit a general(ized) linear model (for example, a linear or logistic regression), we estimate coefficients for each predictor. If the original features were standardized, these coefficients can be used to estimate relative feature importance; larger absolute value coefficients are more important. This method is computationally inexpensive because coefficients are calculated when we fit the model. It is also useful for both classification and regression problems (i.e., categorical and continuous outcomes). However, similar to the other methods described above, these coefficients do not take highly correlated features into account.












----------------------------------


# Hyperparameters
A hyperparameter in ML is a value that determines part of the learning process and is not affected by training. 
In the k-nearest neghbors algorithm, 'k' is a hyperparameter as it determines how many neighbors will be used. 
Decision trees also have hyperparameters, such as the depth of the tree, or the minimum number of samples in a node.
Regularisation factors (alpha) in linear and logistic regression are hyperparameters.


*Hyperparameters tuning* can make or break a model. The balance between overfitting and underfitting a model is called *bias-variance tradeoff*. Being bias the difference between the model's prediction and the correct values, and variance the dependence of a model on training data (high variance performs vell on training data and viceversa)



*Hyperparameter tuning methods*
- Grid search algorithm:
	+ Trains a model on predetermined lists of hyperparameters. It tries every one and uses the one that performs the best
- Random search algorithm:
	+ Similar to before but does not have a list of hyperparameter values
- Bayesian optimisation
	+ Uses bayesian statistics to iterate through the values. Each time the algorithm evaluates a new hyperparameter value it gains more info about where it should look for the best value
- Genetic algorithms:
	+ These go through several generations of hyperparameter, within each generation the best performing values are slightly mutated to produce the next generation.



# Algorithm to hyperparameter
Legend:
- Ml algorithm
	+ scikit-learn package
	+ Hyperparameters
	+ parameters in sklearn
	+ Notes
	+ How to tune


- Linear regression (with gradient descent)
	+ sklearn.linear_model.SGDRegressor
	+ learning rate, regularisation penalty
	+ learning_rate, penalty, alpha
	+ Set penalty to l1 or l2 (default). Larger alpha will result in stronger regularisation
	+ Learning rate and C can be tuned with a grid or random search
	


- Logistic regression
	+ sklearn.linear_model.LogisticRegression
	+ regularisation penalty
	+ penalty, C
	+ Set penalty to l1, l2 (def), or none. C is the inverse of regularisation eg smaller C will result in stronger regularisation
	+ C can be tuned with a grid or random search



- K-nearest neighbors
	+ sklearn.neighbors.KNeighborsClassifier
	+ k (number of neighbors)
	+ n_neighbors
	+ n_neighbors can be tuned with grid or random search



- Decision trees
	+ sklearn.tree.DecisionTreeClassifier
	+ Maximum depth of tree, minimum number of samples split in internal node
	+ max_depth, min_sample_split
	+ Both can be tuned with grid or random search



- k-means
	+ sklearn.cluster.KMeans
	+ k (number of clusters)
	+ n_clusters
	+ use elbow method
	

- Support vector machines
	+ sklearn.svm.SVC
	+ regularisation penalty
	+ C
	+ Smaller C will result in stronger regularisation
	+ C can be tuned with a grid or random search
	




## Grid search algorithm hyperparameter tuning

```py
#This is for logistic regression 
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
 
# Load the data set
X = vert.drop('class', axis=1)
y = vert['class']
 
# Split the data set into training and testing data
X_train, X_test, y_train, y_test = train_test_split(X, y)
 
# Create the logistic regression model
model = LogisticRegression(solver = 'liblinear', max_iter = 1000)
#We use solver 'liblinear' so that we can try different regularisation techniques later on as it is both compatible with l1 and l2


#List of paramters we want to feed in the grid search
parameters = {'penalty': ['l1', 'l2'], 'C': [1, 10, 100]}


# Create grid
from sklearn.model_selection import GridSearchCV
 
clf = GridSearchCV(model, parameters)


#Fit the model
clf.fit(X_train, y_train)


# Now we can see which values performed best
print(clf.best_estimator_)

# -> LogisticRegression(C=1, penalty='l1', solver='liblinear')



# To check how each performed we can check for corresponding indexes:
print(clf.cv_results_['params'])
print(clf.cv_results_['mean_test_score'])
```


## Random search hyperparameter tuning
The random seach literally gets random values and tries them out.



```py
# THE SETTING UP OF A MODEL IS EXACTLY THE SAME AS BEFORE

from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
 
cancer = load_breast_cancer()
 
lr = LogisticRegression(solver = 'liblinear', max_iter = 1000)



# ALTHOUGH THE VALUES ARE RANDOM WE HAVE TO GIVE A RANGE
from scipy.stats import uniform
 
distributions = {'penalty': ['l1', 'l2'], 'C': uniform(loc=0, scale=100)}



# Prepare model
from sklearn.model_selection import RandomizedSearchCV
 
clf = RandomSearchCV(lr, distributions, n_iter=8)


#Fit data
clf.fit(cancer.data, cancer.target)


# Check best parameter
print(clf.best_estimator_)

#-> LogisticRegression(C=81.21687287754932, max_iter=1000, penalty='l1', solver='liblinear')



# To check how each performed we can check for corresponding indexes:
print(clf.cv_results_['params'])
print(clf.cv_results_['mean_test_score'])
```





















