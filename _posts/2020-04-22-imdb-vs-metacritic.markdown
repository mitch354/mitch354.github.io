---
layout: post
title:  "IMDB vs Metacritic"
date:   2020-04-22 07:43:49 -0700
categories: jekyll update
---

### Goal

Compare and contrast film ratings from the Internet Movie Database (IMDB) against Metacritic. IMDB ratings are from users while Metacritic is an average of actual film critics.

### Data Collection

All necessary information (including Metacritic ratings) was available on IMDB. Chose close to 1700 entries from random lists such as [this](https://www.imdb.com/list/ls009796553/). Scraped from there and put into a Pandas dataframe with columns:

`"Name"` - *String* - Name of the film

`"Length"` - *Int* - Length of the film in minutes

`"Year"` - *Int* - The year the film was released

`"Genre"` - *String* - The type of film - if many were listed the first one was chosen

`"MPAA"` - *String* - Rating of the film (R, PG-13, etc.)

`"Budget"` - *Int* - Cost of making the film

`"BoxOffice"` - *Int* - How much the film made in theates

`"IMDB"` - *Int* - Rating of the film on IMDB

`"numRatings"` - *Int* - How many users contributed to the ratings

`"Metacritic"` - *Int* - Rating of the film on Metacritic



Distributions for Year, Genre and MPAA looked as follows:

*Year*

<img src="/imgs/2020-04-22/year_hist.png" title='years'>

*MPAA*

<img src="/imgs/2020-04-22/mpaa.png" title='mpaa'>

*Genre*

<img src="/imgs/2020-04-22/genre.png" title='genre'>


At a quick glance the year and MPAA distributions looked correct to me but the genre seemed off. There were too many comedies and it was odd the the counts were going down in alphabetical order. When I looked at the source data on IMDB I realized that they were actually listing genres for a movie in alphabetical order, not by which was most applicable. With that in mind I decided to drop this column as the current values weren't reliable and I lacked a timely way of properly categorizing them.

### Results

**Average Rating - overall**

<img src="/imgs/2020-04-22/rating_hist.png" title='ratings'>

Metacritic is indeed more critical. Also much greater variance:

IMDB Standard Deviation 0.947

Metacritic Standard Deviation 1.755


**Average Rating - By MPAA**

<img src="/imgs/2020-04-22/violin.png" title='violin'>


| MPAA    | IMDB | Metacritic | Difference   |
| ------------- |-------------|-------------|-------------|
| G | 6.826 | 6.502 | 0.325|
| PG| 6.486 | 5.519 | 0.967|
|PG-13| 6.503 | 5.273 | 1.230|
|R| 6.820 | 5.931 | 0.888|

While Metacritic gives out lower ratings across the board, G rated films are much closer than the others to ratings on IMDB. I believe this is likely due to a different set of standards all movie goers might have with these films. They are often meant to entertain children and as such reviewers will bring their families and evaluate the film on whether or not they've had a good time.

The greater variance found with Metacritic ratings are further illustrated here. The density curves are much less peaked and have longer tails in both extremes; not just low ratings.

**Average Rating - By Decade**

<img src="/imgs/2020-04-22/violin2.png" title='violin2'>

Sorting ratings by decade didn't reveal any new trends. In the next article I'll look at budget and Box Office numbers and see if they have any impact of ratings.
