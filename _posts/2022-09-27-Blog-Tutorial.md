---
layout: post
title:  Stop Studying Dull Stats
date:   2022-09-26
author: Alden
description: A fun and quick how to guide to prepare any dataset for analysis
image: https://github.com/aldenm01/stat386-projects/blob/main/assets/images/BlogCover.jpg
---
 
 # In case a TA sees this before the 30th, this post is not ready to be graded yet.  I have received accommodations and Dr. Tass has given me until Friday (9/30/2022) to finish it.
 
The prophet Nephi in the Book of Mormon preaches to his people to liken the scriptures unto themselves.  I believe as statisticians, we should also liken our statistics unto our own lives.  Statistic students shouldn't be analyzing the proportion of the colors of M&Ms.  There is an infinite world of data at the click of a button.  If you are already going to spend 4 years slaving away in TA Labs, you might as well have fun while you're doing it.  Data wrangling data is one of the most important skills in the statistics field. Mastering it will broaden the data you can analyze exponentially and take your research to the next level.


![image](https://user-images.githubusercontent.com/112586829/193083046-a07f98c3-2fd0-4c60-b293-e90ae78ec103.png)


As a student, I would often have trouble finding data to analyze that I would be interested in.  However, this is where likening statistics unto ourselves makes data wrangling interesting and fun.  In addition to statistics, I also have a love for gaming, so for this example, I have decided to analyze data from one of my favorite Nintendo classics, Fire Emblem.


![Eyecatch_1](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Fire_Emblem_Shadow_Dragon_Blade_of_Light_4.jpg)


If you have not played Fire Emblem, briefly said, it is a tactical role playing game where you control an army of different soldiers with their own personalities
and talents to reclaim the country of Archenea.  If a unit defeats enough enemy soldiers, they gain experience and level up.  Each character has eight stats that have a different percent chance of increasing by one each time they level up, but for the sake of this post, we will focus on strength.  The higher the strength a unit has, the more damage they deal.


![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Fire_Emblem_Battle.png)


## Setting the Stage

Let's try to put this in context.  Let's say I just got hired to create the next Fire Emblem game.  When designing
a cast of characters in a role playing game, you want to be sure they are not too strong so it is not too easy, or too weak, as to be unfairly challenging.  If this fails, you might end up with the unfortunate situation below.  I want to analyze growth rate data for the strength stat for the character on a previous title to do this.  For this example, I will be gathering data on the first Fire Emblem game released in 1990.


![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Wendy.png)
![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Percival.png)


The data I used can be found in the link below.
https://serenesforest.net/shadow-dragon-and-the-blade-of-light/characters/growth-rates/

## With the stage set, let's get wrangling!

## Webscraping:

Probably the easiest part of data wrangling.  I personally love finding data online and analyzing it.  Bottom line, if there is a table on a site, it can be scraped.  The internet has enough information to last a statistician several lifetimes.  This can be done using the the pd.read_html function in pandas.  This is the command I used my table.  You may need to add the second line if you are getting 403 forbidden errors while trying to scrape your data.

url = 'https://serenesforest.net/shadow-dragon-and-the-blade-of-light/characters/growth-rates/'
r = requests.get(url)
BOLGrowths = pd.read_html(r.text)
BOLTable = BOLGrowths[0]
BOLTable.head()

(Note: BOL stands for Blade of Light)

Remember, tables that have been scraped in this method are stored in a list of pandas dataframes that you have to reference.  If there were two tables, I could reference it by entering BOLGrowths[1].


![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Table_1.png)


## Issue 1: Incompatible Data Types in Table

With our table here, it looks like we can use simple python functions to find the average of the strength column with a line such as sum[BOLTable['Str']], but this isn't actually the case.


![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Error_0.png)


This can be solved by running a pd.to_numeric statement like this:
BOLTable['Str'] = pd.to_numeric(BOLTable['Str'])

However, we run into another error...


![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Error_2.png)


## Issue 2: Unwanted Rows

If we take a look at the webpage's data, we can see that there are rows in the table that repeat the column names.  


![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Serenes.png)


This was done intentionally.  Normally, if an internet surfer was looking at this table, he/she may scroll down far enough to obscure the columns, and remembering the columns while looking down the list would be frustrating.  However, this causes errors since we cannot take run pd.to_numeric on strings. We could go through and remove each row individually that has this problem, but I like to use less lines when I can, so here's a more efficient solution. The Str column value for the bad rows has the string, 'Str' as the value, so this gets rid of those rows, much like how grep -v 'argument' works in the command line.

BOLTable = BOLTable[BOLTable['Str'].str.contains('Str')==False] 


![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/Solution_1_Results.png)


After running this, we can see if you compare the old data to this, that row 20 (as well as the other troublesome rows) are gone.  

Now the above code (BOLTable['Str] = pd.to_numeric(BOLTable['Str']) works.  If you compare the table from the data on the link, you can see that it has been removed.  We can now run the above pd.to_numeric statement without error.  If I wanted to do all the columns, that would be easy too.  If you're interested on how this can be done, this page is a great resource.

https://stackoverflow.com/questions/36814100/pandas-to-numeric-for-multiple-columns

## Success!!  A Perfectly Clean Data Set


![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/New_Table.png)


And there we have it.  As we can see, we can now run analysis on this data.  When you start a project where you can gather the data, I urge you to pick something that interests you.  Don't just try to get a simple dataset to finish the assignment.  There's so much great data out there.  As statisticians, we have the great privelage of analyzing everything and anything we want.  Think of any hobby or media you like, such as music, video games, Star Wars, etc., and there is a great opportunity to  showcase your skills to the world and having fun while doing it. 

![Test Image](https://github.com/aldenm01/stat386-projects/blob/main/assets/images/BlogCover.jpg)
