---
author: tr8dr
comments: true
date: 2015-02-28 23:41:11+00:00
layout: post
link: https://tr8dr.wordpress.com/2015/02/28/oxyplot-bitcoin-progress-etc/
slug: oxyplot-bitcoin-progress-etc
title: OxyPlot, Bitcoin progress, etc.
wordpress_id: 1097
categories:
- strategies
---

I've been a bit distracted recently brainstorming some blockchain-related ideas with colleagues, and working on a research & trading UI.

**OxyPlot**

First I wanted to give a plug for OxyPlot.  If you use F#/C# or the .NET ecosystem, [OxyPlot](http://oxyplot.org) is a well designed interactive scientific plotting library.  The library renders to iOS, Android, Mono.Mac, GTK#, Silverlight, and WPF.    If one is dealing with a manageable amount of data, highly recommend [Bokeh](http://bokeh.pydata.org/en/latest/) & python.   However if you need significant interactive functionality or need to interact with large data sets, OxyPlot is a great solution:

[![Screen Shot 2015-02-28 at 6.00.14 PM](https://tr8dr.files.wordpress.com/2015/02/screen-shot-2015-02-28-at-6-00-14-pm.png?w=300)](https://tr8dr.files.wordpress.com/2015/02/screen-shot-2015-02-28-at-6-00-14-pm.png)I wanted to build a UI to allow me to observe and develop better intuitions around order book characteristics.   OxyPlot did not have good financial charts, so contributed high-performance interactive candlestick & volume charts that can handle millions of bars (points) and some pane alignment controls to the project.   Here is an example of the sort of UI one can put together with OxyPlot:

[![Screen Shot 2015-02-28 at 5.53.31 PM](https://tr8dr.files.wordpress.com/2015/02/screen-shot-2015-02-28-at-5-53-31-pm.png?w=660)](https://tr8dr.files.wordpress.com/2015/02/screen-shot-2015-02-28-at-5-53-31-pm.png)

**Feed Status**

I was awaiting a reply from BTCChina regarding their broken FIX implementation.   As the API issue has not been resolved, implemented the BTC China REST API and deployed the feeds for the 4 exchanges of primary interest:

[![Screen Shot 2015-02-28 at 6.23.18 PM](https://tr8dr.files.wordpress.com/2015/02/screen-shot-2015-02-28-at-6-23-18-pm.png)](https://tr8dr.files.wordpress.com/2015/02/screen-shot-2015-02-28-at-6-23-18-pm.png)









Though 2 of the exchanges require 4-sec sampling (due to the lack of streaming APIs), all intervening trades are captured between queries.   For L2 sources, order book transactions are implied by looking at the minimum sequence of transactions that would produce the difference between 2 successive snapshots of the order book.   All of the feeds yield a common transaction format:

[![Screen Shot 2015-02-10 at 6.33.12 PM](https://tr8dr.files.wordpress.com/2015/02/screen-shot-2015-02-10-at-6-33-12-pm.png)](https://tr8dr.files.wordpress.com/2015/02/screen-shot-2015-02-10-at-6-33-12-pm.png)




