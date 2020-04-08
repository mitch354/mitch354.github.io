---
layout: post
title:  "Boxing vs MMA - Part 1"
date:   2020-04-04 15:43:49 -0700
categories: jekyll update
---

### Goal

Compare and contrast top ranked boxers against top ranked MMA fighters. This part will focus on Stance, Age and Record. The next part will dive deeper into strength of competition.

### Data Collection

Used Selenium with Headless Chrome to scrape boxer stats from https://boxrec.com/en/ratings and used MMA data I had already gathered. Put into two Pandas dataframes with features:

```
column_names = ['Weightclass', 'Name', 'Age', 'Stance', 'Wins', 'Losses', 'Draws', 'Link']
boxersDF = pd.DataFrame(columns = column_names)
mmaDF = pd.DataFrame(columns = column_names)
```
`"Weightclass"` - *String* - What weight the fighter was competes at

`"Name"` - *String* - Name of the fighter

`"Age"` - *Int* - Age of the fighter in years

`"Stance"` - *['southpaw', 'orthodox']* - Which foot/hand the fighter leads with

`"Wins"` - *Int* - Number of wins the fighter has accumulated

`"Losses"` - *Int* - Number of losses the fighter has accumulated

`"Draws"` - *Int* - Number of draws the fighter has accumulated

`"Link"` - *String* - For future processing. Links to the fighters Tapology / Box Rec page where their strength of competition may be analized

### Results

***Comparison of Stances between MMA & Boxing***

<img src="/assets/img/2020-04-04/bar.png" title='bar'>

Boxing seems to have a bit more a proclivity towards southpaw fighters but, at 31% probability of the same means, there's not strong enough evidence to confirm. Let's take a closer look by breaking down the distribution by weight class:

<img src="/assets/img/2020-04-04/mmavboxing.png" title='mmavboxing'>

MMA has an outlier in the welterweight division where 9/16 of the fighters in the sample are southpaws. If we take this division out as well as the boxing division with the most southpaws (featherweight) we get:

<img src="/assets/img/2020-04-04/bar2.png" title='bar2'>

With the outliers removed there's a 7.3% chance these datasets have the mean. Still a larger sample size should be taken for an affirmative answer. It would also be interesting to explore if the welterweight division remains an outlier or if this was just a chance sampling.

***Comparison of age between MMA & Boxing***

```Python
print("MMA Normality Test: " + str(stats.normaltest(mmaDF['Age']).pvalue))
print("Boxing Normality Test: " + str(stats.normaltest(boxersDF['Age']).pvalue))
print("Probability of equal variance: " + str(stats.levene(mmaDF['Age'], boxersDF['Age']).pvalue))

MMA Normality Test: 0.03781844203980707
Boxing Normality Test: 0.48667341957628285
Probability of equal variance: 0.9556934435227888
```


The normal test, as it often does, failed for MMA fighters but inspecting the curves reveals that both data sets have somewhat normal shapes.
<img src="/assets/img/2020-04-04/age_hist.png" title='age_hist'>

```Python
print("MMA mean: " + str(mmaDF['Age'].mean()))
print("Boxing mean: " + str(boxersDF['Age'].mean()))
print("Probability of equal means: " + str(stats.ttest_ind(mmaDF['Age'], boxersDF['Age']).pvalue))

MMA mean: 31.80314960629921
Boxing mean: 29.909774436090224
Probability of equal means: 4.56981195468321e-05
```

Top boxers are around two years younger than their MMA counterparts. This aligns with my intuition. Young boxers such as Ryan Garcia (21), Davin Haney (21), and Vergil Ortiz Jr. (22) are already prime to contend in their divisions. It is very rare to see a fighter this age in the UFC, let alone ranked. Most at this age probably only have a handful of fights.

***Comparison of fight numbers between MMA & Boxing***

*Total Fights*

The data is a little less normal here but the distributions are similarly shaped and it's clear that the average boxer has more fights than the average MMA fighter. This is more revealing when you consider the age discrepancy described above. I'd need additional data to shed more light on it but I expect boxers are starting earlier and packing in a ton of fights in their early careers. Once guys hit the elite level, i.e the ones chosen for this survey, 2 fights a year is the standard convention.

<img src="/assets/img/2020-04-04/hist2.png" title='hist2'>

*Win Ratio*

<img src="/assets/img/2020-04-04/hist3.png" title='hist3'>

These distributions could not be more distinct and seem to echo the common sentiment that the best don't fight the best in boxing. A more in depth breakdown of this will be presented in the next article.
