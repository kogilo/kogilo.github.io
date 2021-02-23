---
layout: post
title: "Linear Regression in Python"
subtitle: "Understanding Linear Regression and its application in predictive  modeling."
background: "/img/posts/machineLearning/linearReg/photo-1495592822108-9e6261896da8.jpeg"
---

## Linear Regression

    - is the simplest and most widely used statistical technique for predictive modeling.
    - is represented mathematically as in the following equation:

![LR](/img/posts/machineLearning/linearReg/SRL.png)

<h2>Training a classifier on Rocks vs Mines</h2>

```python
import numpy
import random
from sklearn import datasets, linear_model
from sklearn.metrics import roc_curve, auc
import pylab as pl

```

<h2> Read the Data</he>

```python
import pandas as pd
from pandas import DataFrame
url="https://archive.ics.uci.edu/ml/machine-learning-databases/undocumented/connectionist-bench/sonar/sonar.all-data"
df = pd.read_csv(url,header=None)
df.describe()

```

<h4>Convert labels R and M to 0 and 1</h4>

```python
df[60]=np.where(df[60]=='R',0,1)
```

<h4>Divide the dataset into training and test samples</h4>
<h4>Separate out the x and y variable frames for the train and test samples</h4>

```python
from sklearn.model_selection import train_test_split
train, test = train_test_split(df, test_size = 0.3)
x_train = train.iloc[0:,0:60]
y_train = train[60]
x_test = test.iloc[0:,0:60]
y_test = test[60]
y_train
```

<h2>Build the model and fit the training data</h2>

```python
model = linear_model.LinearRegression()
model.fit(x_train,y_train)
```

<h1>Interpreting categorical prediction results</h1>
<h3>Precision
<h3>Recall
<h3>True Positive Rate
<h3>False Positive Rate
<h3>Precision recall curve
<h3>ROC curve
<h3>F-Score
<h3>Area under PR curve
<h3>Area under ROC curve

<h4>Generate predictions in-sample error</h4>

```python
training_predictions = model.predict(x_train)
print(np.mean((training_predictions - y_train) ** 2))

```

```python
print('Train R-Square:',model.score(x_train,y_train))
print('Test R-Square:',model.score(x_test,y_test))

```
