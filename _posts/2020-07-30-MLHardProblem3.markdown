---
author: tr8dr
comments: true
date: 2020-07-30 10:00:00+00:00
layout: post
title: Why ML → Finance is Hard  (3 / 4)
subtitle: The problem of non-independent samples
categories:
- machine learning
---
Following on from the [prior post](https://tr8dr.github.io/MLHardProblem2/), want to discuss the problem of __sample independence__.
Many machine learning models in finance deal with timeseries data, where samples used in training may be __close together in
time and not be independent of one another__.  There are very few features in finance that do not make use of lookback periods.  Most 
features do evaluate prior windows:

- almost all technical indicators (SMA being the most basic example)
- distribution based signals
- decomposition based signals
- traditional signal processing
- volatility: rolling stdev, garch, instantaneous vol, etc
- hawkes processes for estimation of variance or buy/sell imbalance
- regime models
- ...

The lookback period used in these features allows a feature at time ``t`` to __overlap__ (and not be i.i.d) with respect to the __last
k features__ (where k is the lookback window size on one's features).  The t .. t+k features, future in the sequence, also have
overlap.  In general, if a given feature has lookback length k (for example a k period MA), the sample will share information
with k prior samples and k future samples (or 2k) samples. 

Some of the best supervised machine learning approaches employ sampling during the process of training, for example:

- deep learning models
- random forest
- genetic algorithms
- ...

When samples lack inter-sample independence (i.e. are not i.i.d temporally), the machine learning model is often __able to exploit the lookahead 
bias__ introduced, overfitting the model in training.  Find some discussion of [this problem here](http://people.ee.duke.edu/~lcarin/IJCAI07-121.pdf).

## Discussion
In our earlier random forest toy example we achieved perfect accuracy during training.  This was mostly due to the lack of
independence across samples, where the __model learned from the lookahead bias__ due to the overlapping ROC windows across
samples.  This overfitting resulted in poor out of sample (OOS) performance.

Applying the same to a deep learning model (such as a LSTM) will achieve similar results, with very high in-sample performance
and poor OOS performance.

Random forest does allow for a number of constraints which can __reduce the impact of non-independent samples__ by:

- __decreasing the # of samples used in fitting each tree in the forest__
  * If the # of random samples (with replacement) per tree is reduced from N (the total # of samples) to M < N, the probability
    of a given sample overlapping with another within the same tree is reduced from nearly 100% to a smaller probability of overlap.
- __reducing the complexity of each tree__ (for example tree depth)
  * this just reduces the number of rules that can exploit the lookahead bias.

These just partially side-step the issue.  A better solution would be to remove the independence issue entirely.

## A solution for Random Forest 
I tend to use bayesian models and random-forest when applying supervised learning, as they are often more appropriate for my 
feature sets than deep learning or alternative approaches.   In terms of adjusting for non-independence, I  
modified scikit-learn's ```RandomForestClassifier``` and ```RandomForestRegressor``` algorithms to address this problem.  

The change was as follows: adjusted the random forest classifier and regressor to allow for a user defined sampling 
function.  This capability allows the user to account for samples that are not i.i.d relative to each other, ensuring 
that each tree contains independent samples.  Find the [modified sklearn library](https://github.com/tr8dr/scikit-learn) here. 
 

Recall that our prior model had perfect accuracy in training (a sure sign of overfitting):

/          | Positive | Negative | Accuracy |
-----------|----------|----------|---------|
Positive   |     1723 |      0   | 100%    |
Negative   |        0 |     2057 | 100%    |

and an out of sample outcome with 47% precision (more losing trades than winning):

/          | Positive | Negative | Accuracy |
-----------|----------|----------|---------|
Positive   |     242 |      269   | 47%    |
Negative   |        526 |     587 | 53%    |


By introducing a sampling that avoids overlapping samples:

```python
def select_stride(random, nrows, samples, skip=20):
    offset = random.randint(0,skip)
    raw = pd.Series(random.randint(0,nrows/skip,samples))
    indices = raw.apply(lambda x: min(x+offset, nrows-1))
    return indices

clf = RandomForestClassifier(
    n_estimators=500, random_state=1, n_jobs=-1,
    sampling_function=select_stride)

model = clf.fit (training[features], training.label)
pred_train = model.predict(training[features])
pred_test = model.predict(testing[features])
```

we get the following confusion matrices.  For in-sample:

/          | Positive | Negative | Accuracy |
-----------|----------|----------|---------|
Positive   |      806 |      867 | 48%     |
Negative   |      917 |     1190 | 56%     |

and out-of-sample with a 51% accuracy:

/          | Positive | Negative | Accuracy |
-----------|----------|----------|----------|
Positive   |      260 |      250 | 51%      |
Negative   |      508 |      606 | 54%      |

Our feature set and labels in the [toy example](https://tr8dr.github.io/MLHardProblem1/) were not brilliant, so did not expect
a workable strategy.  That said, by removing the sampling overlap we:

- reduced training model overfit (as evidenced above with the accuracy < 100%)
- improved out-of-sample performance

## Conclusions
- Temporal non-independence of samples can degrade ML models substantially, causing bias and overfit
- Training algorithms need to be modified to remove the impact of non-independent samples




