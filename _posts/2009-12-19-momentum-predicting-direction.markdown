---
author: tr8dr
comments: true
date: 2009-12-19 01:25:31+00:00
layout: post
link: https://tr8dr.wordpress.com/?p=392
published: false
slug: momentum-predicting-direction
title: Momentum Predicting Direction
wordpress_id: 392
categories:
- strategies
---

MACD seems to be an effective measure when paired with other analysis.   We need to look at:



	
  1. **derivative of the lightly smoothed MACD**
Crossings precede inflections generally

	
  2. **Momentum largely over with inflection in MACD
Slowed momentum indicates high chance of following inflection in price series. **

	
  3. **observe slope of MACD to determine whether momentum is over**
When MACD inflects should exit position as momentum lost

	
  4. **use volatility to scale time, removing ewma issues**
Duration RV seems OK, but expectation based may work better.

	
  5. **scaling MACD timing based on volatility
Alternatively we may want to descale the time axis, and normalize all assets to some vol level. **


**Here are some variables**:



	
  1. MACD level (smoothed)

	
  2. dMACD

	
  3. prior / current MACD inflection +,-

	
  4. amplitude from prior non-trivial inflection

	
  5. prior window slope

	
  6. current window slope

	
  7. volatility

	
  8. S&P direction



**Some Examples**

[![](http://tr8dr.files.wordpress.com/2009/12/picture-15.png)](http://tr8dr.files.wordpress.com/2009/12/picture-15.png)

[![](http://tr8dr.files.wordpress.com/2009/12/picture-24.png)](http://tr8dr.files.wordpress.com/2009/12/picture-24.png)

**Uses Random Trees**



	
  1. Determine trade training set, values between -1, 0, 1

	
  2. Determine relevant variables with random trees

	
  3. Attempt to use random trees to predict portfolio



** **

