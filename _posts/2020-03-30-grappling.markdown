---
layout: post
title:  "Grappling Analysis - Best Fights and Promotions"
date:   2020-03-30 15:43:49 -0700
categories: jekyll update
---

### Goal

Systematically rank the MMA fights based on their grappling components. Activity and competitiveness are the main attributes desired here. There are many great grappling performances that were quick and one sided but here I'm looking for fights where both fighters had some level as success.

### Data Collection

Used Selenium with Headless Chrome to scrape fight stats from [ufcstats.com](http://www.ufcstats.com/statistics/events/completed). Put into a Pandas dataframe with features:

```Python
column_names = ["Weightclass", "Submission", "Rounds", "Date", "A_name", "A_takedowns", "A_takedown_attempts", "A_ground_strikes", "A_ground_strike_ratio", "A_sub_attempts", "A_passes", "A_reversals", "B_name", "B_takedowns", "B_takedown_attempts", "B_ground_strikes", "B_ground_strike_ratio", "B_sub_attempts", "B_passes", "B_reversals"]
```

`"Weightclass"` - *String* - What weight the fight was held at

`"Submission"` - *Boolean* - Whether or not the fight ended with a Submission

`"Rounds"` - *int* - How many rounds the bout lasted

`"Date"` - *datetime* - When the bout took place

`"A_name"` - *String* - the name of Fighter A

`"A_takedowns"` - *Int* - The number of takedowns completed by fighter A

`"A_takedowns_attempts"` - *Int* - The number of takedowns attempted by fighter A (complete + incomplete)

`"A_ground_strikes"` - *Int* - Number of strikes landed by fighter A while on the ground

`"A_ground_strike_ratio"` - *Float* - A_ground_strikes / total number of strikes

`"A_sub_attempts"` - *Int* - The number of Submission attempts attempted by fighter B

`"A_passes"` - *Int* - The number of times fighter A passed fighter B's guard

`A_reversals` - *Int* - The number of times fighter A swept or reversed fighter

The columns for fighter B follow the above definitions

Fights with this form were gathered separately for the UFC, WEC, Strikeforce and Pride organizations and merged together when needed for analysis.

### Data Cleaning & Engineering

# Calculating Grapple Score

```Python
def getGrappleScore(fight):
    A_score = 1
    A_score += 0.75 * fight.A_takedowns
    A_score +=  (1/4) * fight.A_takedown_attempts
    A_score += 1.75 * fight.A_passes
    A_score += 3 * fight.A_reversals
    A_score += 2.5 * fight.A_sub_attempts
    A_score += (0.2 * fight.A_ground_strike_ratio) * fight.A_ground_strikes
    B_score = 1
    ### ...
    ### Same as above
    ### ...
    B_score += (0.2 * fight.B_ground_strike_ratio) * fight.B_ground_strikes
    Fight_Score = fight.Submission * 4 + (A_score + B_score) * (1 - ((max(A_score,B_score) - min(A_score,B_score))**1.5/max(A_score,B_score)**1.5)) * (1 - fight.Rounds * .05)
    return Fight_Score

ufc_df['Score'] = ufc_df.apply(lambda X: getGrappleScore(X), axis=1)
wec_df['Score'] = wec_df.apply(lambda X: getGrappleScore(X), axis=1)
strikeforce_df['Score'] = strikeforce_df.apply(lambda X: getGrappleScore(X), axis=1)
pride_df['Score'] = pride_df.apply(lambda X: getGrappleScore(X), axis=1)
```

****  
<br/>
```Python
A_score = 1
A_score += 0.75 * fight.A_takedowns
A_score +=  (1/4) * fight.A_takedown_attempts
A_score += 1.75 * fight.A_passes
A_score += 3 * fight.A_reversals
A_score += 2.5 * fight.A_sub_attempts

```

A_score/B_score are started at 1 to avoid a potential division by 0 error. Otherwise the individual scores are just a linear combination of the fighter's grappling stats with different weights. Reversals are highly rewarded because they indicate an exchange in which both fighters had some level of success. Submission attempts, passes, takedowns and takedown attempts were likewise scaled by how much I believed they contributed to an active grappling match.

```Python
A_score += (0.2 * fight.A_ground_strike_ratio) * fight.A_ground_strikes
```

Ground strikes, being the easiest stat to accumulate, were a bit more difficult to factor in and were scaled down to no dominate the score. In lieu of being able to find stats regarding total time on the ground I also used ground_strike_ratio in this term as an approximation of top control time (the fighter on top usually lands significant strikes at a much higher rate). If a fighter has a high ground strike ratio it's likely they spent a significant portion of the fight on top of their opponent instead of on the bottom or just standup.

```Python
Fight_Score = fight.Submission * 4 + (A_score + B_score) * (1 - ((max(A_score,B_score) - min(A_score,B_score))**1.5/max(A_score,B_score)**1.5)) * (1 - fight.Rounds * .05)
```

`fight.Submission * 4` - Rewarding a fight for ending in a Submission

`(A_score + B_score)` - Sum of both fighters scores

`((max(A_score,B_score) - min(A_score,B_score))**1.5/max(A_score,B_score)**1.5)` - Returns a number in [0.0, 1.0], the closer to 1 the closer the fight scores for each fighter were. I found the term on its own was too punishing and the term squared wasn't enough so settled at raising the term to the power of 1.5.

`(1 - fight.Rounds * .05)` - Fights with more rounds will naturally have more stats accumulated so this term makes the fight score consider that.

### Results

**Top 25 Grappling Fights:**

| Fighter A     | Fighter B   | Score  |
| ------------- |-------------| -----|
|Louis Smolka|Tim Elliott|52.593|
|Nate Diaz|Alejandro Garcia|49.244|
|John Alessio|Alex Serdyukov|47.303|
|Russell Doane|Jerrod Sanders|46.976|
|Ed Herman|Kendall Grove|45.07|
|Magomed Magomedov|Petr Yan|44.818|
|Chris Wade|Islam Makhachev|44.595|
|Antonio Rodrigo Nogueira|Bob Sapp|44.47|
|Michinori Tanaka|Kyung Ho Kang|44.219|
|Matt Grice|Jason Black|40.897|
|Kevin Holland|Gerald Meerschaert|40.41|
|John Hathaway|Rick Story|40.199|
|Cub Swanson|Micah Miller|39.002|
|Carlos Condit|Jake Ellenberger|38.847|
|Heath Herring|Cheick Kongo|38.615|
|Chase Beebe|Rani Yahya|38.329|
|Dan Henderson|Yuki Kondo|38.309|
|Quinton Jackson|Murilo Rua|38.096|
|Demian Maia|Jason MacDonald|36.977|
|Paddy Holohan|Louis Smolka|36.839|
|Gerald Meerschaert|Oskar Piechota|36.689|
|Ricardo Lamas|Dave Jansen|36.466|
|Dan Henderson|Murilo Rua|36.355|
|Jared Hamman|Rodney Wallace|36.267|
|Ben Rothwell|Gilbert Yvel|36.13|

**Grappling Score by Promotion:**

<img src="/assets/img/2020-03-30/heatmap.png" title='heatmap'>

<img src="/assets/img/2020-03-30/histogram.png" title='histogram'>

All promotions share a very similar, extremely right skewed distributions; close to a geometric. This is a product of many fights having no grappling components at all or if they do, then they're extremely one sided. Pride & WEC have noticeably higher means and this aligns with my 'eyeball' test.

Pride's events took place in the early to mid 2000's when the grappling meta wasn't close to where it is today. WEC mostly featured fights in the lighter weight classes, 155lbs and lower, where physical attributes naturally lead to more scrambles and exciting grappling exchanges.
