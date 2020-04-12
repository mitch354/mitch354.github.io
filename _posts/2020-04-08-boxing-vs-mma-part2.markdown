---
layout: post
title:  "Boxing vs MMA - Part 2"
date:   2020-04-07 07:43:49 -0700
categories: jekyll update
---

### Goal

Compare and contrast top ranked boxers against top ranked MMA fighters. This part will focus on strength of competition; analyzing the quality of opponents ranked boxers and MMA fighters have faced to date.

### Data Collection

Same DataFrames as before with some additions:

`"OppWins"` - *Int* - Sum of the number of wins the opponents had at the time of the matchups

`"OppDraws"` - *Int* - Sum of the number of draws the opponents had at the time of the matchups

`"OppLosses"` - *Int* - Sum of the number of losses the opponents had at the time of the matchups

`"RankedOpps"` - *Int* - Number of opponents fought who are currently ranked

To get these I scraped data from individual fighter pages on Boxrec and Tapology eg:

[Tyson Fury](https://boxrec.com/en/proboxer/479205)

[Conor McGregor](https://www.tapology.com/fightcenter/fighters/14607-conor-mcgregor)

### Results

We left off the last post with this comparison of win ratios between MMA and Boxing:

<img src="/assets/img/2020-04-04/hist3.png" title='hist3'>

The disparity in the shapes between the distributions led me to wanting to more closely compare the records of MMA fighters and boxers.

***Comparing quality of opponents***

*Opposition's Record*

Plotted here are the sum of each of fighter's opponents wins and losses at the time that they fought:

<img src="/imgs/2020-04-08/scatter.png" title='scatter'>

The regression lines are remarkably similar in slope and the difference in intercept makes sense because as we saw in the last article the average ranked boxer has ~3 more fights. There is a massive difference in variance though. Amongst ranked fighters UFC athletes tend to have faced opponents with similar records overall on the way up. This can very widely from boxer to boxer though.

I think more than anything this disparity speaks to the level of control a boxer has over their career compared to MMA fighters. The attitude in MMA is 'the best fight the best' and 'anytime, anywhere'. With more promoters, sanctioning bodies, etc. in boxing their allegiance is more to themselves and they can, to some extent, matchmake how they see fit.

There were several notable outliers with respect to High Opponent Losses, Low Opponents wins in the boxing plot so I decided to take a closer look:

<img src="/imgs/2020-04-08/scatter-named.png" title='scatter-named'>

The outliers are all British fighters. This was an unexpected finding but after looking at these fighters individually I saw a pattern. Their first 10 or so fights were filled with fighters with TERRIBLE records. Kell Brook's first opponent was 31-189. After these first 10 or so fights they start fighting better opposition.



*Ranked Opponents*

<img src="/imgs/2020-04-08/boxplot.png" title='boxplot'>

It comes as little surprise that MMA fighters are fighting other ranked guys more often then their counterparts in boxing. With different sanctioning bodies, promotions and broadcasters these fights don't come together as easily as they do in the UFC's monopolistic business. This is all despite having fewer total fights on average.

This is a product of MMA fighters usually reaching the highest level of competition with far fewer fights than boxers. Boxers tend to take many, lower level, build up fights in the early stages of their career. MMA fighters in contrast have a more linear build up when it comes to competition level.
