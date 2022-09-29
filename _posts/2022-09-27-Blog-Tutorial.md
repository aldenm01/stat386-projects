---
layout: post
title:  Stop Studying Dull Stats
date:   2022-09-26
author: Alden
description: A fun and quick how to guide to prepare any dataset for analysis
image: https://user-images.githubusercontent.com/112586829/193084903-f030efe8-63d2-48ab-9d82-0a571d332221.png
---

## Make Stats Fun for You!

The prophet Nephi in the Book of Mormon preaches to his people to liken the scriptures unto themselves.  I believe as statisticians, we should also liken our statistics unto our own lives.  Statistic students shouldn't be analyzing the proportion of the colors of M&Ms.  There is an infinite world of data at the click of a button.  If you are already going to spend 4 years slaving away in TA Labs, you might as well have fun while you're doing it.  Data wrangling data is one of the most important skills in the statistics field. Mastering it will broaden the data you can analyze exponentially and take your research to the next level.


![image](https://user-images.githubusercontent.com/112586829/193083046-a07f98c3-2fd0-4c60-b293-e90ae78ec103.png)


I often have trouble finding data to analyze that I find interesting.  However, this is where likening statistics unto ourselves makes data wrangling engaging and fun.  For example I love gaming, so for this example, I have decided to analyze data from one of my favorite Nintendo classics, Fire Emblem.


![image](https://user-images.githubusercontent.com/112586829/193084294-b12ff40f-bcf6-403c-9805-dcf89a03ebd8.png)


If you have not played Fire Emblem, briefly said, it is a tactical role playing game where you control an army of different soldiers with their own personalities
and talents to reclaim the country of Archenea.  If a unit defeats enough enemy soldiers, they gain experience and level up.  Each character has eight stats that have a different chance of increasing by one each time they level up, but for the sake of this post, we will focus on strength.  The higher the strength a unit has, the more damage they deal.


![image](https://user-images.githubusercontent.com/112586829/193084325-fd75b850-d9bd-4498-90aa-359375820758.png)


## Setting the Stage

Let's try to put this in context.  Let's say I just got hired to create the next Fire Emblem game.  When designing a cast of characters in a role playing game, you want to be sure they are not too strong so it is not too easy, or too weak, as to be unfairly challenging.  If this fails, you might end up with the unfortunate situation below.  I want to analyze growth rate data for the strength stat for the character on a previous title to do this.  For this example, I will be gathering data on the first Fire Emblem game released in 1990.


![image](https://user-images.githubusercontent.com/112586829/193084378-1f41ce7e-7cf9-4812-b23d-afb7d369c52c.png)
![image](https://user-images.githubusercontent.com/112586829/193084400-2c3bd1c5-2afe-4282-a4a8-8b5ae22894d3.png)


The data I used can be found in the link below.
https://serenesforest.net/shadow-dragon-and-the-blade-of-light/characters/growth-rates/

## With the stage set, let's get wrangling!

## Webscraping:

This is likely the easiest part of data wrangling.  I personally love finding data online and analyzing it.  Bottom line, if there is a table on a site, it can be scraped.  The internet has enough information to last a statistician several lifetimes.  This can be done using the the pd.read_html function in pandas.  This is the command I used my table.  You may need to add the second line if you are getting 403 forbidden errors while trying to scrape your data.

url = 'https://serenesforest.net/shadow-dragon-and-the-blade-of-light/characters/growth-rates/'
r = requests.get(url)
BOLGrowths = pd.read_html(r.text)
BOLTable = BOLGrowths[0]
BOLTable.head()

(Note: BOL stands for Blade of Light)

Remember, tables that have been scraped in this method are stored in a list of pandas dataframes that you have to reference.  If there were two tables, I could reference it by entering BOLGrowths[1].


![image](https://user-images.githubusercontent.com/112586829/193084482-40212276-5b90-4627-a7a4-3d2cf8076eab.png)


## Issue 1: Incompatible Data Types in Table

With our table here, it looks like we can use simple python functions to find the average of the strength column with a line such as sum[BOLTable['Str']], but this isn't actually the case.


![image](https://user-images.githubusercontent.com/112586829/193084528-d53ae63d-5e44-4fa9-9e72-249e9d9a77be.png)


This can be solved by running a pd.to_numeric statement like this:
BOLTable['Str'] = pd.to_numeric(BOLTable['Str'])

However, we run into another error...


![image](https://user-images.githubusercontent.com/112586829/193084572-7ff7f7fd-8589-4f77-8e89-f59c63db494d.png)


## Issue 2: Unwanted Rows

If we take a look at the webpage's data, we can see that there are rows in the table that repeat the column names.  


![image](https://user-images.githubusercontent.com/112586829/193084616-313fe6b7-4280-4acd-9cfe-705b42b9d46a.png)


This was done intentionally.  Normally, if an internet surfer was looking at this table, he/she may scroll down far enough to obscure the columns, and remembering the columns while looking down the list would be frustrating.  However, this causes errors since we cannot take run pd.to_numeric on strings. We could go through and remove each row individually that has this problem, but I like to use less lines when I can, so here's a more efficient solution. The Str column value for the bad rows has the string, 'Str' as the value, so this gets rid of those rows, much like how grep -v 'argument' works in the command line.

BOLTable = BOLTable[BOLTable['Str'].str.contains('Str')==False] 


![image](https://user-images.githubusercontent.com/112586829/193084787-9a46cd53-2a00-44e1-ae78-cfb612f0b39d.png)


After running this, we can see if you compare the old data to this, that row 20 (as well as the other troublesome rows) are gone.  

Now the above code (BOLTable['Str] = pd.to_numeric(BOLTable['Str']) works.  If you compare the table from the data on the link, you can see that it has been removed.  We can now run the above pd.to_numeric statement without error.  If I wanted to do all the columns, that would be easy too.  If you're interested on how this can be done, this page is a great resource.

https://stackoverflow.com/questions/36814100/pandas-to-numeric-for-multiple-columns

## Success!!  A Perfectly Clean Data Set


![image](https://user-images.githubusercontent.com/112586829/193084842-7624ff2a-bc61-4f19-967a-877c6c37dd6d.png)


And there we have it.  As we can see, we can now run analysis on this data.  The next time you can gather your own data for a project, I urge you to pick something that interests you.  Don't just try to get a dull dataset to finish the assignment.  There's so much interesting data out there.  As statisticians, we have the great privilege of analyzing everything and anything we want.  Think of any hobby or media you like, such as music, video games, Star Wars, etc., and there is a great opportunity to  showcase your skills to the world and having fun doing it. 

![image](https://user-images.githubusercontent.com/112586829/193087388-115f55b4-5854-46d9-925e-3357eb3f411e.png)
