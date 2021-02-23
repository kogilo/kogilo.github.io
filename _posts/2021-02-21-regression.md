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

## Read the Data

```python
import pandas as pd
from pandas import DataFrame
url="https://archive.ics.uci.edu/ml/machine-learning-databases/undocumented/connectionist-bench/sonar/sonar.all-data"
df = pd.read_csv(url,header=None)
df.describe()

```

## Convert labels R and M to 0 and 1

```python
df[60]=np.where(df[60]=='R',0,1)
```

- Divide the dataset into training and test samples
- Separate out the x and y variable frames for the train and test samples

```python
from sklearn.model_selection import train_test_split
train, test = train_test_split(df, test_size = 0.3)
x_train = train.iloc[0:,0:60]
y_train = train[60]
x_test = test.iloc[0:,0:60]
y_test = test[60]
y_train
```

## Build the model and fit the training data

```python
model = linear_model.LinearRegression()
model.fit(x_train,y_train)
```

## Generate predictions in-sample error

```python
training_predictions = model.predict(x_train)
print(np.mean((training_predictions - y_train) ** 2))

```

```python
print('Train R-Square:',model.score(x_train,y_train))
print('Test R-Square:',model.score(x_test,y_test))

```
