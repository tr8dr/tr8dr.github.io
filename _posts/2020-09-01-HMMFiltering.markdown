---
author: tr8dr
comments: true
date: 2020-09-01 11:00:00+00:00
layout: post
title: Denoising a signal with HMM
subtitle: 
categories:
- indicators
---
Most signals I deal with are noisy, reflecting noise of underlying prices, volume, vol of vol, etc.  Many traditional
strategies built on such indicators might either:

- use signal to scale into position
  * such approaches have to deal with noise to avoid thrashing, adjusting position up and down with noise
- consider specific levels of the signal to signify a state
  * for example: long {+1}, short {-1}, neutral {0}
  
Pictured below is an example of a noisy signal that detects downward momentum.  The goal is to signal 1 when in
downward momentum and 0 when not.  A __simplistic approach__ might be to __set a threshold__ and map
the raw signal to 1 when above threshold andto 0 when below the threshold.   

<img src="/assets/2020-09-01/noise.png" width="800" height="550" />

In the above scenario, there is no threshold level that would avoid unwanted oscillation between 0 and 1 states. 
For example, with the threshold set at 0.75 there are 2 incursions below the threshold (which would be mapped to state 0).  Trying
other thresholds, such as 0.5, avoiding noise during the momentum period, but would encounter noise thereafter.   

The denoised signal (pictured as the green line), makes use of a hidden markov model (HMM).  Have used this for the past
10 years or so, so thought to share the approach here.

## A Solution
In the application above we are interested in assigning states (or levels) +1 and 0, removing the noise from our signal.
Let's consider a common scenario where we want to assign { Short, Neutral, Long } states to a noisy signal.

To do this can construct a 3 state system and assign transition probabilities, for example probability of transitioning from
Long to Short, Neutral to Long, remaining in the same state, etc.

<img src="/assets/2020-09-01/state-system.png" width="700" height="400" />

One can think of the same state probability as defining the "stickiness" to a given state, and resistance to noise that 
might otherwise cause us to transition to another state.  For example if we assign a high probability to $$ P_{short \rightarrow short} $$,
and a corresponding lower probability to transitions out of that state, we will be able to ride more noise without transitioning
to another state.

The state system above gives us a transition matrix:

$$
\begin{bmatrix}
P_{short \rightarrow short} & P_{short \rightarrow neutral} & P_{short \rightarrow long}\\ 
P_{neutral \rightarrow short} & P_{neutral \rightarrow neutral} & P_{neutral \rightarrow long}\\ 
P_{long \rightarrow short} & P_{long \rightarrow neutral} & P_{long \rightarrow long} 
\end{bmatrix}
$$

Note that the probabilities must have the relationship that the sum across each row must = 1, i.e. for transition
matrix M: $$ \sum_j M_{i,j} = 1 $$.  If one wants to __define the denoising model in terms of "stickiness" to same state__, then can determine the probabilities
for the transition matrix as:

$$
\begin{bmatrix}
P_{short \rightarrow short} & \frac{1}{2} (1 - P_{short \rightarrow short}) & \frac{1}{2} (1 - P_{short \rightarrow short})\\ 
\frac{1}{2} (1 - P_{neutral \rightarrow neutral}) & P_{neutral \rightarrow neutral} & \frac{1}{2} (1 - P_{neutral \rightarrow neutral})\\ 
\frac{1}{2} (1 - P_{long \rightarrow long}) & \frac{1}{2} (1 - P_{long \rightarrow long}) & P_{long \rightarrow long} 
\end{bmatrix}
$$

Next we need to consider how the __noisy signal is mapped to these states__.  The approach that HMM takes is to
have the notion of __observation distributions__ $$ p(y \vert x) $$, where $$ y $$ are our observations (the raw signal in this
case) and $$ x $$ is the particular "hidden state".  

Hence we want to design or otherwise determine observation distributions that provide the likelihood of being in a particular
state.  Given a symmetric signal between -1 and +1, we could design the observation distributions as follows (I will show for
the 2 state long/short for visual clarity):

<img src="/assets/2020-09-01/observation-dist.png" width="750" height="550" />

By positioning the observation distributions near +1 and -1 observing a raw signal sequence $$ y_n, y_{n-1}, .. y_0 $$, we
can observe the likelihood that $$ y_i $$ belongs to either the long state or short state, defined by:

- long state: $$ p(y \vert x = long) = N(+0.65, \sigma) $$ or
- short state $$ p(y \vert x = short) = N(-0.65, \sigma) $$.

The observation distribution provides us with $$ p(y \vert x = s) $$, but what we are looking for is 
$$ p(x = s \vert y_n, y_{n-1}, .. y_0) $$, i.e. the probability of being on state "s" given the observation 
sequence (our noisy signal), $$ y_n, y_{n-1}, .. y_0 $$.

The HMM model ties the transition probability matrix $$ M $$, our observation probability distributions $$ p(y \vert x = s) $$, and
state probability priors \[1\] $$ { \pi_{s = short}, \pi_{s = neutral}, \pi_{s = long} } $$ into a __model which determines the most
likely state as we proceed along the sequence of observations__. (The priors are typically determined as the frequency of each state such as 1/3, 1/3, 1/3).

As concisely [described here](https://en.wikipedia.org/wiki/Forward_algorithm), at each time step in the sequence we
the likelihood for being on each state as:

$$
\alpha_t(x_t) = p(y_t \vert x_t) \sum_{x_{t-1}} p(x_t | x_{t-1}) \alpha_{t-1}(x_{t-1})
$$

where $$ p(x_t \vert x_{t-1}) $$ is for any state pair (i,j) our transition probability as defined by $$ M $$ and 
$$ p(y_t \vert x_t) $$ is our observation distribution for given state at $$ x_t $$.  For t = 0, the priors $$ \pi_s $$ are used
instead of the transition matrix, as at t = 0, we do not yet have an initial state.  

The above is decomposed into a dynamic programming problem and computed in log-likelihood space to avoid underflow.  I can 
go into the implementation in another note if there is interest.

### Filtering (summary)
In summary, the approach to mapping a Long / Short signal or alternative configuration of states is to:

- define the observation distributions in a way that separates the signal space, distinguishing states
- define a transiton probability matrix controlling "stickiness" (amount of state transition noise)
- define priors (typically )
- apply to raw signal with above configuration to forward viterbi model

Here is an example (which I did not attempt to optimise):

```python
import pandas as pd
from tseries_patterns.ml.hmm import HMM2State
from tseries_patterns.common import scale_x_datetime_auto
from tseries_patterns.data import YahooData

from talib import ADX
import plotnine
from plotnine import *

# get price bars
yahoo = YahooData()
aapl = yahoo.getOHLC("AAPL", Tstart='2019-1-1')

# compute our raw signal (not a very good one, FYI)
rawsignal = ADX(aapl.high, aapl.low, aapl.close, timeperiod=20)

# denoise signal
hmm = HMM2State (means = [10, 40], ss_prob = 0.9999)
## predict 0 and 1 states, rescaling to 10, 40 to align with scale of ADX
denoised = 10 + hmm.predict(rawsignal.dropna()) * 30

# plot
prices = pd.DataFrame({'date': aapl.index, 'price': aapl.close, 'pane': ' price', 'what': 'price'})

metrics = pd.concat([
    pd.DataFrame({'date': rawsignal.index, 'value': rawsignal, 'pane': 'indicator', 'what': 'raw signal'}),
    pd.DataFrame({'date': denoised.index, 'value': denoised, 'pane': 'indicator', 'what': 'denoised'}),
])

plotnine.options.figure_size = (10,8)
(ggplot() + 
     geom_line(aes(x='date', y='price'), color='darkgrey', data=prices) +
     geom_line(aes(x='date', y='value', color='what'), data=metrics) +
     scale_x_datetime_auto (prices["date"], (10,8)) +
     facet_grid('pane ~ .', scales='free_y'))
```

The above `HMM2State` class explicitly defines:

- the transition probability matrix:
  * $$ i = j \rightarrow 0.9999 $$ and
  * $$ i \neq j \rightarrow 1 - 0.9999 $$.
- the observation distributions as:
  * $$ S_0 = N(\mu = 10, \sigma_{default}) $$ and
  * $$ S_1 = N(\mu = 40, \sigma_{default}) $$.
- the prior probabilities as $$ \pi_i = \frac{1}{2} $$

The forward viterbi method is then used to determine the states in the `hmm.predict()` call.

<img src="/assets/2020-09-01/ADX-denoised.png" width="800" height="550" />

## Alternatives (that don't work well)
A typical approach with HMM involves using the __forward-backward algorithm__ to automatically determine:

- transition probabilities
- observation distributions
- priors

For example:

```python
from hmmlearn.hmm import GaussianHMM

rawsignal = ...
model = GaussianHMM(n_components=3)
model.fit (rawsignal)

states = model.decode(rawsignal)
```

While this will find 3 states, is unlikely not to have the desired filtering effect:

- the 3 observation distributions may be skewed by biases in the raw signal
- the transition probabilities are unlikely to represent the desired "stickness" and hence denoising desired.

In general you will achieve better results by defining the observation distributions and transition probability matrix
yourself.