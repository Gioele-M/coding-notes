# Supervised learning 3
These models are built on top of the others


# Support Vector Machines
SVM is a powerful supervised model used for _classification_. SVM makes classifications by defining a decision boundary and then seeing what side of the boundary an unclassified point falls on. The boundaries are defined with a training set of classified points.
One problem SVM encounters is figuring out which decision boundary use as there could be an infinite number that separates correctly two classes. In general it should maximise the distance between the decision boundary and points.
To do this we use _support vectors_, these are the points in the training set closest to the decision boundary, in fact they decide the boundary positioning. They're called vectors even though they're points because are considered as vectors coming from the origin of the axis (0,0).
If you're using n features, there are at least n+1 support vectors.
The distance between a support vector and the decision boundary is called _margin_, which we want to make sure is as large as possible.
This brings to ignore many other training points, which can be an advantage as only focuses on the relevant defining points.




_In SkLearn_
```py
from sklearn.svm import SVC
from graph import points, labels

classifier = SVC(kernel = 'linear', C=1) # C can go to 0.01

#classifier = SVC(kernel = "rbf", gamma = 0.5, C = 2)

classifier.fit(points, labels) 

print(classifier.predict([[3, 4], [6, 7]]))
```

*C*
Outliers are an issue for this type of algorithm. To solve this the SVC classifier has the parameter _C_ which determines how much error is allowed. If it is high then it won't allow for many misclassifications and the margin could be small (could overfit data). If it is small some points might fall on the wrong side of the line and the margin would be too large. This helps with outliers but it risks underfitting the model. (Ideally you test different values of C)


*Kernels*
The kernel determines what type of relation the model should look for. In this case linear but could be also `classifier = SVC(kernel='poly', degree=2)`
Polynomial kernels add another dimension to our data as in 
`(x, y) -> (sqrt(2)•x•y, x^2, y^2`
This transformation makes the data linearly separable in a 3D plane and allows for fitting the model properly.

_The most commonly used kernel is radial basis function (rbf)_. This is also the defalut kernel in SVC. This transforms the points into an infinite number of dimension.
`classifier = SVC(kernel = "rbf", gamma = 0.5, C = 2)`
The _gamma_ parameter is similar to C. You can tune the model to be more or less sensitive to the training data. A high gamma (ex 100), will put importance on the training data and overfit. A low gamma (ex 0.01), makes the points less relevant and could underfit.








-------------------------





# Random forest
Random forest is an ensemble machine learning technique consisting of many decision trees that work together to classify new points. When required to classify a point, the forest gives the point to each tree and returns the most popular classification. (Great solution against overfitting)

*Bagging*
Decision trees are deterministic, meaning that given a training set, the same tree will be made every time. Random forrest create different trees through bagging eg. every time a tree is made, it is created using a different subset of the points in the training set, this way each tree is different. We do this process with replacement (putting the rows back in the df), this is because even if we pick the same row more than once and we would still end up with a unique data set.
*To increment variety we can also bag subsets of _features_*. A good way of bag features is using the squared root of the amount of features per subset.


*Classification*
As mentioned before the classification result is the mode of the decision forest eg most frequent value.


*In scikit*
```py
from sklearn.ensemble import RandomForestClassifier

classifier = RandomForestClassifier(n_estimators = 2000, random_state = 0) #estimators is number of trees, random_state is to test the code

classifier.fit(training_points, training_labels)

classifier.score(testing_points, testing_labels)


prediction = classifier.predict(point, label)

```




------------




# Naive Bayes Classifier

*Bayes' Theorem*
Bayes' theorem is the basis of the Bayesian statistics, and was born to crack ww2 codes.
The theorem works by determining if two events are dependent or independent.
Conditional probability is the probability that two events happen. If the two events are independent the probability of them happening is = P(A) * P(B).

_Cross probability_
Example: testing for a rare disease (1 in 100,000), and the test is 99% accurate. Given that the test is 1% wrong, but we actually know the proportion of manifestation, if we get a positive result there are two possibilities:
- Probability of patient has the disease
	+ Account for the accuracy and disease rate 
	+ eg rate disease * accuracy test -> (1/100000) * 0.99
- The patient doesn't have it and the test was wrong
	+ Account for the inaccuracy and the non-disease rate
	+ eg rate healthy * inaccuracy -> (99999/100000) * 0.01


_Bayes' theorem_
If instead we wanted to calculate the probability that a patient has the disease _given_ the test came back positive, we would need to focus on the probability of an event happening _in succession to another_ P(A|B) eg probability of A happening, _given_ B has happened. The formula is:
`P(A|B) = (P(B|A)•P(A))/P(B)` 
eg.
P(rare disease|positive result) = (P(positive result|rare disease)•P(rare disease))/P(positive result)

_To calculate_ P(positive result|rare disease) we have to take into account that the test is 99% accurate in detecting the disease. Given that the patient HAS the disease, this results being 99%
_The P(rare disease)_ is only the probability of the disease (eg 1/100000)
_The P(positive result)_ we have to sum *both* the probability of the patient having the disease and the test being accurate (eg 1/100000 * 0.99) AND the probability of the patient NOT having the disease and the test being wrong (eg 99999/100000 * 0.01)


## Naive Bayes classifier
The Naive bayes classifier leverages Bayes' theorem to make predictions and classifications.
The formula `P(A|B) = (P(B|A)•P(A))/P(B)` (probability of A happening given B), can be translated into a classifier if we replace B with a _data point_ and A with a _class_.
Ex. we're trying to classify an email as either spam or not spam. 
We could calculate P(spam | email) and P(not spam | email). Whichever probability is higher will be the classifier’s prediction.
This is considered a supervised algorithm because the model has to compute the probability of the events before being able to make a prediction.


_In SciKit learn_
Before doing this we need to transform the data in a suitable format, using CountVectorised. This has to be trained for vocabulary with the .fit() method. This will transform the string inputed into a vector of numbers based on the dictionary it produced from training.


```py
from reviews import neg_list, pos_list
from sklearn.feature_extraction.text import CountVectorizer

review = "This crib was amazing"

#Instantiate the vectoriser
counter = CountVectorizer()

# Fit all data together
counter.fit(neg_list + pos_list)

#Check the developed vocabulary
#print(counter.vocabulary_)

#Transform the review variable in a vector
review_counts = counter.transform([review]) #!! takes in a list
#Check it
#print(review_counts.toarray())

# Transform the lists of reviews into vectors
training_counts = counter.transform(neg_list + pos_list)

```

*Now we can use the MultinomialNB classifier*. This takes in an array of data points and an array of labels corresponding to each data point. The .predict() method will predict the labels of the new points given a list of points. The .predict_proba() method instead will return the _probability_ of each label given a point.

```py
from sklearn.naive_bayes import MultinomialNB

review = "This crib was trash, absolutely horrendous"
review_counts = counter.transform([review])

#Instantiate classifier
classifier = MultinomialNB()

#We have to create a list of labels, given that we trained neg_list and pos_list combined, the first half should be 0 and the second half should be 1 (we know how many given the lenght of the dataset)
training_labels = [0]*1000 + [1]*1000

classifier.fit(training_counts, training_labels)

#Check for classification on transformed review
print(classifier.predict(review_counts))

#Check for probability that the review is good or bad
print(classifier.predict_proba(review_counts))

```

To improve the model:
- Remove puntuation from the training set
- Lowercase every word in the training set
- Use a bigram or trigram model (eg words are evaluated in groups of 2 or 3 instead of 1 at the timen)





























