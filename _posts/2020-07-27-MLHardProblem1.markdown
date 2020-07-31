---
author: tr8dr
comments: true
date: 2020-07-27 17:00:00+00:00
layout: post
title: Why ML → Finance is Hard (1/4)
subtitle: Why is it hard to apply machine learning to trading?
categories:
- machine learning
---
I have used __machine learning__ in trading strategies over the past 10 years or so.  However my use of ML has 
often played a relatively small role in the overall design and success of the strategies due to issues particular
to financial data sets.   I tend to use ML in specific signals or strategy sub-problems where the data / problem setup 
have attributes that lead to a robust statistical solution.  This is as opposed
to the "Nirvana" scenario where fundamental features and objective are provided to an AI and the AI generates functioning 
strategies with little effort on the part of the researcher. 

I was part of a very well funded AI startup that used machine learning on a massive scale to discover
new and novel medium frequency trading strategies.  More often than not, a strategy determined 
in training failed to generalize and behave as expected in testing, validation, or live.   We spent considerable time and
research understanding and improving on the problems of applying ML to trading over the years.  Subsequent to this have
used machine learning in prop trading, market making, and in derivatives modeling at other firms.   
 
As have had a few learnings from these experiences, hopefully some that are useful to others, will share here:  

## Problems
Financial timeseries present some of the most difficult problems for machine learning.  Here I will list a
few of the challenges with financial data and why it is so difficult to model with ML.

1. __Low signal to noise__:
   * label noise
   * features and measures are noisy variables
     
2. __Feature vector non-independence__
   * Many machine learning algorithms require that each sample (feature vector / label) be *independent of other
     samples*.
   * Many features make use of windows, creating overlap between the current sample and prior samples sharing part 
     of the same lookback period.
   * This can have a devastating effect on training, causing model bias and overfitting.
   
3. __Standard loss functions not geared for trading objectives__
   * We have *much higher aversion to losses* (false-positives) than to *opportunity cost* (false-negative), whereas ML
     models try to balance precision and recall.
   * Many supervised machine learning algorithms need to be adjusted towards a suitable trading objective.
   
3. __Sparsity of opportunities__ (for some strategies):
   * implication: unbalanced data sets for supervised learning
   * very often a trading opportunity is an "outlier", a much less frequent event in the data.  Most machine learning
     algorithms are geared for balanced data sets.
     
4. __Non-stationarity__
   * Prices not stationary and returns are memory-less, need to balance between the two
   * Timescales are not stationary with respect to patterns (patterns can play out over shorter or longer timescales)
   * Regime change presents shift in fundamental trading patterns -> complexity for training models
   
5. __Lack of data or biased data__
   * Depending on frequency of data or pattern one is pursuing, financial data is often orders of magnitudes less
     plentiful than data sets in other fields.
   * Markets can be in 1 regime for an extended period of time (witness the equity bull markets), biasing available
     data towards a single regime.  


There are ways to deal with some of these issues.  I first want to highlight the problems, illustrating with examples. 

## Toy Example
Suppose we want to use ML to create a long-only model to predict whether the 5 day return > some minimum return (say 50bps).  

1. Create labels, identifying trading opportunities 
   * We will label 5 day returns >= 50bps with 1 (where we should enter) and < 50bps with 0 (where we should skip entry).  
2. Create features for the model
   * Note that I don't advocate using technical indicators generally, and the features below are just for illustration 
     purposes, not chosen intentionally.  
3. Train the model
4. Evaluate

__See full code in the appendix__

```python
bars = getOHLC ("SPY")
close = bars["Adj Close"].values.flatten()

# create features
df = bars.copy()
df["rsi"] = talib.RSI(close, timeperiod=14)
df["roc5"] = talib.ROC(close, timeperiod=5)
...

# create { 0, 1 } labels, where 1 means 5 day return >= 50bps
df["label"] = (df["roc5"].shift(-5) >= 0.50) * 1.0

# feature columns
features = ["rsi", "roc1", "roc5", "roc10", "roc20", "oc", "hl"]

# split data set
icut = int(df.shape[0] * 0.70)
training = df.iloc[:icut].dropna()
testing = df.iloc[icut:].dropna()

# train model on features & labels
clf = RandomForestClassifier(n_estimators=500, random_state=1, n_jobs=-1)
model = clf.fit (training[features], training.label)

# use model to predict labels for training period and testing period respectively
pred_train = model.predict(training[features])
pred_test = model.predict(testing[features])

```

Now lets look at the confusion matrix for training and testing through the model we trained.  For trading
we want to maximize TP (true-positive, our profitable trades) and minimize FP (false positives, out losing
trades):

/          | Positive | Negative |
-----------|----------|----------|
Positive   |       TP |       FP |
Negative   |       FN |       TN | 

For trading we are less concerned with TN (true negative) and FN (false negative).  False negatives would
represent missed opportunities, but no loss.

Now let's evaluate the confusion matrices for the model given the training data and then the testing data.  Our
hope is that the testing (out-of-sample) data presents an accuracy similar to training and that the ratio of
TP to FP is high.

```python
confusion_matrix(training.label, pred_train)
confusion_matrix(testing.label, pred_test)
```
It turns out that our training period has a perfect fit (FN = 0 and FP = 0).  This is a sure sign of overfitting:

/          | Positive | Negative |
-----------|----------|----------|
Positive   |     1721 |        0 |
Negative   |        0 |     2057 | 


And in testing (out-of-sample) has a poor precision where 271 of (271 + 241) trades are losing.

/          | Positive | Negative |
-----------|----------|----------|
Positive   |      241 |      271 |
Negative   |      529 |      582 | 


## Discussion
The above example illustrates a number of problems we will discuss in subsequent posts:

- __Labeling is noisy__ (noisy labeling problem)
  * We have assigned +1 labels to returns >= 50bps, however some of these returns may not be representative of an underlying
    move, rather be a presentation of noise around the true price.  For example, the true price return over a period 
    could be 0, but have a variance of 50bps or more. 
  * We have assigned 0 labels to returns < 50bps, however in some cases the underlying price process may be yielding >= 50bps
    over the period, but the sample return is reduced below 50bps due to volatility around a 50bps+ mean. 
  * Hence we are pushing the model to map features to noise in some % of instances and biasing the model
  
- __Features are noisy__
  * Our open-close and high-low features, in particular, will suffer from volatility
  
- __Samples not independent__
  * Each row in our feature set is not independent from neighboring rows.  Our longest feature has a lookback window of
    20 bars.  Hence every feature row will have overlap with 40 other features (20 in the past + 20 in the future).  Many
    ML algorithms will exploit the information leakage due to non-independence, creating an overfit model in training.
  * There are changes we can make to RandomForest, for example, which will help us overcome this problem.
  
- __Biased data set__
  * The data set used in this example is primarily showing prices rising over time (due to the "unnatural" and continual rise 
    of the SPY over the last 10+ years).  It would be better to have data which expresses equal weighted upward and downward
    trends. 
  
- __Wrong loss objective__
  * We would prefer to optimize for a better balance between TP and FP (precision), as opposed to balancing precision
    and recall.
  
Will discuss these in more depth in subsequent posts, and pose some adjustments that can be done to algorithms, datasets, etc.

## See Next
- [part 2](https://tr8dr.github.io/MLHardProblem2/)
- [part 3](https://tr8dr.github.io/MLHardProblem3/)


## Appendix 

```python
import talib
import pandas as pd
import numpy as np
from datetime import datetime
from pandas_datareader import data as pdr
from sklearn.metrics import confusion_matrix
from sklearn.ensemble import RandomForestClassifier

def getOHLC (stock: str, Tstart = datetime(1999,1,1), Tend = datetime.now()):
    raw = pdr.get_data_yahoo([stock], start=Tstart, end=Tend)
    df = raw[["Open","High","Low","Close","Adj Close","Volume"]]
    return df

bars = getOHLC ("SPY")
close = bars["Adj Close"].values.flatten()

# create features
df = bars.copy()
df["rsi"] = talib.RSI(close, timeperiod=14)
df["roc1"] = talib.ROC(close, timeperiod=1)
df["roc5"] = talib.ROC(close, timeperiod=5)
df["roc10"] = talib.ROC(close, timeperiod=10)
df["roc20"] = talib.ROC(close, timeperiod=20)
df["oc"] = np.log(bars.Close / bars.Open)
df["hl"] = np.log(bars.High / bars.Low)

# create { 0, 1 } labels, where 1 means 5 day return >= 50bps
df["label"] = (df["roc5"].shift(-5) >= 0.50) * 1.0

# feature columns
features = ["rsi", "roc1", "roc5", "roc10", "roc20", "oc", "hl"]

# split data set
icut = int(df.shape[0] * 0.70)
training = df.iloc[:icut].dropna()
testing = df.iloc[icut:].dropna()

# train model on features & labels
clf = RandomForestClassifier(n_estimators=500, random_state=1, n_jobs=-1)
model = clf.fit (training[features], training.label)

# use model to predict labels for training period and testing period respectively
pred_train = model.predict(training[features])
pred_test = model.predict(testing[features])

print(confusion_matrix(training.label, pred_train))
print(confusion_matrix(testing.label, pred_test))

# note that this is an overestimate, since may double count overlapping +1 label 5 day periods
print("P&L estimate: %1.0f%%" % (pred_test * testing.rfwd).sum())

```

