# AI vs ML vs DS

### AI:
Artificial intelligence is an area of computer science, where the goal is to enable computers and machines to perform human-like tasks and simulate human behaviour


### DS:
Data science is a field that attempts to find patterns and draw insights from data (might use ML) 


### ML:
Machine learning is a subset of AI that tries to solve a specific problem and make prediction using data
- Supervised learning:
	+ Uses labelled inputs (meaning the input has corresponding output label) to train models and learn outputs
		* The labels of the input are referred to as feature vector, which are fed into the model to produce an output (prediction)
		* Features types:
			- Qualitative - categorical data (ex gender)
				+ Nominal (no inherent order)
					* We use ONE-HOT encoding (each label is recognised by a specific string/array of numbers)
				+ Ordinal (inherent order ex ratings/age groups)
					* We use inherent category to categorise
			- Quantitative - numerical valued data
				+ Discrete
				+ Continuous
				
	+ TASKS That can be accomplished
		* Classification 
			- Prediction of discrete classes
			- Multiclass OR binary classifications
		* Regression
			- Prediction of continuous values
	
	+  Models:






- Unsupervised learning:
	+ Uses unlabelled data to learn about patterns

- Reinforcement learning:
	+ Agent learning in interactive environment based on rewards and penalties





# Machine learning

## Resources
UCI - machine learning repository -> Offers datasets


# Demo OF CLASSIFICATION
MAGIC gamma telescope dataset from UCI
(in data folder magic04.data)

Description: Predict type of particle reflecting on the telescope based on the attributes of the particles


## Step 1 - Prepare the data

Open Jupyter notebook and start with imports
```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

Given that there are no column labels, these need to be assigned 
```
#Assign labels to columns
cols = ['fLength', 'fWidth', 'fSize', 'fConc', 'fConc1', 'fAsym', 'fM3Long', 'fM3Trans', 'fAlpha', 'fDist', 'class']
# Read DF
df = pd.read_csv('magic04.data', names=cols)
```

The first issue is that there are categorical attributes represented by letters, so this has to be changed to numbers







## Step 2 - Objective

We want to classify the data based on its features in order to assign a label (class) to each instance




























