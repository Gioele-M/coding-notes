# Unsupervised learning
Unsupervised Learning describes a class of algorithms that find patterns from unlabelled or untagged data. In Supervised Learning, we deal with data that is labelled or tagged. However, often training data isn’t labelled properly and this is where unsupervised learning comes in! It relies on using the underlying distributions of features within the data to figure out clusters of similarity. Main 3 are:
- Clustering
- Dimensionality reduction
- Automated labelling










# K-Means clustering
Clustering is the most famous unsupervised learning technique. It finds structure in unlabeled data by identifying similar groups (clusters). Examples include image segmentation, text clustering, search engines (group new topics and search results). 
The goal is to separate the data in groups with similar characteristics. In the _k-means_ clustering we address _k_ as the number of clusters we expect to find and _means_ as the average distance of data to each cluster (aka centroid) that we're trying to minimise.
The approach is:
- Place *k* random centroids for the initial clusters
- Assign data samples to the nearest centroid
- Calculate new centroids based on the above-assigned data samples
- Repeat step 2 and 3 until convergence
Convergence occurs when points don't move between clusters and centroids stabilise. This is the _training process_.
Once we define the clusters, we can take a new unlabeled datapoint and assign it to a cluster. This is defined as _inference_.
(Might be difficult to define how many clusters we're looking for eg _k_)



_Example on iris dataset_
```py
import matplotlib.pyplot as plt
from sklearn import datasets

#iris dataset with 3 types of iris
iris = datasets.load_iris()
# Sepal length - sepale width - petal length - petal width (for respective columns)

#Check data
print(iris.data)

#These are the assigned values that we don't have in real life but we can use it to assess the model
print(iris.target)

#description of data
print(iris.DESCR)

# Store iris.data
samples = iris.data

#get column 0 -> x = samples[:,0]
# [:,0] can be translated to [all_rows , column_0]



# Process the data!!
from sklearn.cluster import KMeans

# Use KMeans() to create a model that finds 3 clusters
model = KMeans(n_clusters=3)

# Use .fit() to fit the model to samples
model.fit(samples)

# Use .predict() to determine the labels of samples 
labels = model.predict(samples)

# Print the labels
print(labels)


# Convert the numeric labels into actual labels
new_names = [iris.target_names[label] for label in labels]

print(new_names)





# Make a scatter plot of x and y and using labels to define the colors
x = samples[:,0]
y = samples[:,1]

plt.scatter(x, y, c=labels, alpha=0.5)
plt.xlabel('sepal length (cm)')
plt.ylabel('sepal width (cm)')
 
plt.show()





#EVALUATION (only doable if you have actual groups really)
#Cross tabulation to check performance
species = [iris.target_names[t] for t in list(target)]

df = pd.DataFrame({'labels': labels, 'species': species})

#Crosstab 
ct = pd.crosstab(df['labels'], df['species'])

print(ct)

species     setosa  versicolor  virginica
labels                                   
setosa           0          48         14
versicolor      50           0          0
virginica        0           2         36

#Virginica didn't do so well

```


*Inertia* measures how spread out are the clusters, it is the distance from each sample to the centroid of its cluster. The lower the inertia, the better it is. However this varies with the number of clusters so there has to be a trade-off.
`print(model.inertia_)`
*Check the range of inertia on graph*
```py
import numpy as np
import pandas as pd
from sklearn import datasets
from sklearn.cluster import KMeans

iris = datasets.load_iris()

samples = iris.data

# Code Start here:
num_clusters = [x for x in range(1,9)]
inertias = []

#Get a range of inertias
for k in num_clusters:
  model = KMeans(n_clusters=k)
  model.fit(samples)
  inertias.append(model.inertia_)


#Plot the whole
plt.plot(num_clusters, inertias, '-o')
 
plt.xlabel('Number of Clusters (k)')
plt.ylabel('Inertia')
 
plt.show()
```





-------------------

# PCA
Principal component analysis is a technique where we can reduce the number of features in a dataset without losing informations.
PCA works by checking the variance of variables in the model, it then 'agglomerates' them into predictors for the model.

PCA takes all the features and calculates a covariance matrix. This is essentially how much a feature changes with changes in every other feature. This will result in a covariance matrix that shows relationships for the entire dataset (with variable variance in the diagonal).

As we have the matrix, the next step is _matrix factorisation_. The goal is to find a pair of smaller matrices whose product would equal our covariance matrix (eg find smaller matrix to capture most of our info). 

For this we use _Eigenvectors_, these are vectors (eg with direction and magnitude) that do not change direction when a transformation is applied to them. These allow to keep a direction in the dataset when looking from a simplified prespective.

*The principal components* are a linear combination of all the input features from the original dataset. These can be 'flattened' from n-dimensional to 2-dimensional space.

*The new obtained 2D dataset could be input in other machine learning models such as linear regression*

*PCA* is also inherently an unsupervised algorithm that can find clusters on its own.

*PCA* is also good for image processing

Principal Component Analysis is a powerful tool that provides a mechanism to reduce dimensionality and simplify datasets without losing the valuable information they inherently contain


_In python scikit_
We have to standardise the data matrix, then perform eigendecomposition by fitting the standardized data. We can access the eigenvectors using the components_ attribute and the proportional sizes of the eigenvalues using the explained_variance_ratio_ attribute.

```py
import numpy as np
import pandas as pd
from sklearn.decomposition import PCA
import codecademylib3

data_matrix = pd.read_csv('./data_matrix.csv')

# 1. Standardize the data matrix
mean = data_matrix.mean(axis=0)
sttd = data_matrix.std(axis=0)
data_matrix_standardized = (data_matrix - mean) / sttd
print(data_matrix_standardized.head())

# 2. Find the principal components
pca = PCA()
components =  pca.fit(data_matrix_standardized).components_
components = pd.DataFrame(components).transpose()
components.index =  data_matrix.columns
print(components)

# 3. Calculate the variance/info ratios
var_ratio = pca.explained_variance_ratio_
var_ratio = pd.DataFrame(var_ratio).transpose()
print(var_ratio)
```

Once we have performed PCA and obtained the eigenvectors, we can use them to project the data onto the first few principal axes. We can do this by taking the dot product of the data and eigenvectors, or by using the sklearn.decomposition.PCA module

_Visualise data divided by PCA_
```py
import numpy as np
import pandas as pd
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt
import seaborn as sns

data_matrix_standardized = pd.read_csv('./data_matrix_standardized.csv')
classes = pd.read_csv('./classes.csv')['Class']

#Transform the data into 4 new features using the first PCs
pca = PCA(n_components=4)
data_pcomp = pca.fit_transform(data_matrix_standardized)
#make data in df and add columns
data_pcomp = pd.DataFrame(data_pcomp)
data_pcomp.columns = ['PC1', 'PC2', 'PC3', 'PC4']
print(data_pcomp.head())


##Plot the first two principal components colored by the bean classes
data_pcomp['bean_classes'] = classes
sns.lmplot(x='PC1', y='PC2', data=data_pcomp, hue='bean_classes', fit_reg=False)
plt.show()
```



### PCA as features
So far we only projected the data onto the PCA to check the subsets. We now could use part of the projected data for modelling, while retaining most of the information of the original dataset.
For example in a dataset where 4 principal axes contained 95% of the total variance, we could use this to train a model instead of 10+ features. This would improve running times and reduce redundancy for better performances.
In this example, we will be using the first four principal components as our training data for a Support Vector Classifier (SVC). We will compare this to a model fit with the entire dataset (16 features) using the average likelihood score. Average likelihood is a model evaluation metric; the higher the average likelihood, the better the fit.

The steps:
- Transform the original data by projecting it onto the first four principal axes. We chose four PCs because we previously found that they contain 95% of the variance in the original data
- Split the data into 77% training and 33% testing sets
- Use the transformed training data to fit an SVM model
- Print out the average likelihood score for the testing data
- Re-split the original 16 standardized features into training and test sets
- Fit the same SVM model on the training set with all 16 features
- Print out the average likelihood score for the test data
- Notice that the score for the model using the first 4 principal components is higher than for the model that was fit with the 16 original features. We only needed 1/4 of the data to get even better model performance!


```py
import pandas as pd
from sklearn.decomposition import PCA
from sklearn.svm import LinearSVC
from sklearn.model_selection import train_test_split
 
 
data_matrix_standardized = pd.read_csv('./data_matrix_standardized.csv')
classes = pd.read_csv('./classes.csv')
 
# We will use the classes as y
y = classes.Class.astype('category').cat.codes
 
# Get principal components with 4 features and save as X
pca_1 = PCA(n_components=4) 
X = pca_1.fit_transform(data_matrix_standardized) 
 
# Split the data into 33% testing and the rest training
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
 
# Create a Linear Support Vector Classifier
svc_1 = LinearSVC(random_state=0, tol=1e-5)
svc_1.fit(X_train, y_train) 
 
# Generate a score for the testing data
score_1 = svc_1.score(X_test, y_test)
print(f'Score for model with 4 PCA features: {score_1}')
 
# Split the original data intro 33% testing and the rest training
X_train, X_test, y_train, y_test = train_test_split(data_matrix_standardized, y, test_size=0.33, random_state=42)
 
# Create a Linear Support Vector Classifier
svc_2 = LinearSVC(random_state=0)
svc_2.fit(X_train, y_train)
 
# Generate a score for the testing data
score_2 = svc_2.score(X_test, y_test)
print(f'Score for model with original features: {score_2}')
```








---------

### PCA for images
An image can be represented as a row in a data matrix where each feature corresponds to the intensity of a pixel.

```py
import numpy as np
from sklearn import datasets
import codecademylib3
import matplotlib.pyplot as plt
 
 
# Download the data from sklearn's datasets
faces = datasets.fetch_olivetti_faces()['data']
 
# Standardize the images using the mean and standard deviation
faces_mean = faces.mean(axis=0)
faces_std = faces.std(axis=0)
faces_standardized = (faces - faces_mean) / faces_std


# 1. Instantiate a PCA object and fit the standardized faces dataset
pca = PCA(n_components=40) 
pca.fit(faces_standardized)

# 2. Retrieve and plot eigenvectors (eigenfaces)
eigenfaces = pca.components_ 

fig = plt.figure(figsize=(10, 8))
fig.suptitle('Eigenvectors of Images (Eigenfaces)')
for i in range(15):
    # Create subplot, remove x and y ticks, and add title
    ax = fig.add_subplot(3, 5, i + 1, xticks=[], yticks=[])
    ax.set_title(f'Eigenface: #{i}')
    
    # Get an eigenvector from the current value of i
    eigenface = eigenfaces[i]

    # Reshape this image into 64x64 since the flattened shape was 4096
    eigenface_reshaped = eigenface.reshape(64, 64)

    # Show the image
    ax.imshow(eigenface_reshaped, cmap=plt.cm.bone)
plt.show()

# 3. Reconstruct images from the compressed principal components
# The principal components are usually calculated using `faces_standardized @ principal_axes` or the `.transform` method
principal_components = pca.transform(faces_standardized) 

# The `inverse_transform` method allows for reconstruction of images in the original size
faces_reconstructed = pca.inverse_transform(principal_components)

# Plot the reconstructed images 
fig = plt.figure(figsize=(10, 8))
fig.suptitle('Reconstructed Images from Principal Components')
for i in range(15):
    ax = fig.add_subplot(3, 5, i + 1, xticks=[], yticks=[])
    ax.set_title(f'Reconstructed: {i}')

    reconstructed_face = faces_reconstructed[i]
    reconstructed_face_reshaped = reconstructed_face.reshape(64, 64)
    ax.imshow(reconstructed_face_reshaped, cmap=plt.cm.bone)
plt.show()

```


















