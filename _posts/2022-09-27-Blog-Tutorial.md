---
layout: post
title:  Becoming Dangerous in the Infinite World of Information
date:   2022-09-26
author: Alden
description: A fun and quick how to guide to prepare any dataset for analysis
image: 
---
 
Being a Statistics major, I often have to do projects that involve working with many different data sets.  These can include the wind speed at certain reference sites
in order to predict the speed of the candidate site, or the effect of restaurant ratings and the price of their food.  These was not exciting for me to analyze 
personally, but I recognize their importance.  However, one of the reasons I enjoy statistics is for the potential one has to analyze the infinite amount of data
available for free on the web.  This is why I do not understand why when many of my fellow students have the opportunity to take a sample of data, they decide to
sample the frequency of colors from M&M's.  I can see why Data Wrangling can be frustrating.  In this article, I am going to show some common errors that can happen when data wrangling, and the fear of all coding beginners, joining tables in the Pandas library in Python.

For this example, I have decided to analyze data from something near to my heart, Fire Emblem.

![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Fire_Emblem_Shadow_Dragon_Blade_of_Light_4.jpg)

If you have not played Fire Emblem, briefly said, it is a tactical role playing game where you control an army of different soldiers with their own personalities
and talents to reclaim the country of Archenea.  If a unit defeats enough enemy soldiers, they gain experience and level up.  Each character has eight stats that have a different percent chance of increasing by one each time they level up, but for the sake of this demonstration, we will focus on strength.  The higher the strength a unit has, the more damage they deal.

![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Fire_Emblem_Battle.png)

### Setting the Stage

As Nephi from the Book of Mormon said we should liken the scriptures unto ourselves, I believe we should also liken the knowledge
of statistics that we learn unto ourselves. Let's try to put this in context.  Let's say I just got hired to create the next Fire Emblem game.  When designing
a cast of characters in a role playing game, you want to be sure they are not too strong as you don't want the game to be too easy, or too weak, as to make the game unfairly challenging.  I want to collect strength growth rate data on previous titles in order to do this.  For this example, I will be gathering data on the first Fire Emblem game released in 1990, and its remake, Fire Emblem: Shadow Dragon, released in 2008.  All data I used can be found in the links below.

https://serenesforest.net/shadow-dragon-and-the-blade-of-light/characters/growth-rates/

### Webscraping:

Probably the easiest part of data wrangling.  I personally love finding data online and analyzing it.  The internet has enough information to last a statistician several lifetimes.  This can be done using the the pd.read_html function in pandas.  This is the command I used
in order to get my table, and the head of that table.  You may need to add the line about
requests if you are getting 403 forbidden errors.

url = 'https://serenesforest.net/shadow-dragon-and-the-blade-of-light/characters/growth-rates/'
r = requests.get(url)
BOLGrowths = pd.read_html(r.text)
BOLTable = BOLGrowths[0]
BOLTable.head()

Remember, tables that have been scraped in this method are stored in a pandas dataframe that you have to reference like this.  If there are multiple tables, you can reference them by increasing this number (Ex: BOLGrowths[1])

Name	HP	Str	Skl	Wlv	Spd	Lck	Def	Res
0	Marth	90	50	40	30	50	70	20	0
1	Caeda	50	20	70	80	90	70	20	0
2	Jagen	10	10	10	0	10	0	0	0
3	Cain	90	30	60	60	60	50	20	0
4	Abel	70	40	50	70	50	40	20	0

### Issue 1: Incompatible Data Types in Table

With our table here, it looks like we can simply use some python functions in order  to find the average of the strenght column with a line such as sum[BOLTable['Str']], but this isn't actually the case.  See the error below.

