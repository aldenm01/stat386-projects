---
layout: post
title:  Statistics for Gamers: A Fire Emblem Dataset
date:   2022-10-21
author: Alden
description: A brief summary of how I combined data from a video game for game design research.
image: https://user-images.githubusercontent.com/112586829/197297566-24c3e4f4-a966-43b8-8207-bd3b93b6736a.png
---

Datasets are blood of a statistician.  Just like how musicians can't perform without scores, statisticians can't explore data without datasets.  What if there are no dataset of what you want to research?  In this case, data scientists must make their own.  When creating a dataset (especially when combining tables from several different sources), there are countless ways that it can go wrong and tiny details that statisticians have to keep in mind.  I will be showing techniques of how to manipulate data in a dataset, as well as problems that can occur during the webscraping process.  

![image](https://user-images.githubusercontent.com/112586829/197303980-1f9c257e-ae22-4ada-9996-f492c8ce74d2.png)

## A Brief Recap

The dataset I created is a joined dataframe of the characters in the classic Nintendo series, Fire Emblem.  I explained the basic premise of the game in my previous blog post, but in case you haven't read it, Fire Emblem is a tactical role playing game where you control an army of soldiers each with their own unique designs, personalities, and strengths.  As they defeat enemies, they level up over time.  Each unit has several stats that have a different percent chance of increasing by one.  I have attached a visual for clarity.  In this image, this cavalier has just leveled up and has leveled up his Hit Points (80% chance), Strength (60% chance), and Spd (40% chance).  Each character has a different spread of percentages for their growth rates.

![image](https://user-images.githubusercontent.com/112586829/197303006-1a749c1b-b131-4269-b7f5-af2a79414152.png)

Data was combined with tables from https://serenesforest.net/

### Why make a dataset like this?  What is the motivation?

As a huge fan of both data exploration and game design, I want to be able to compare different variables between games to better understand how the developers balanced the difficulty.  By making a dataset of my own, I have a convenient way to import the data into a program and explore it myself.  Since this data has been public for several years, I see no trouble webscraping it with the requests package in python.

### Part 1: The Basics

For most of the tables in the dataset, I used the requests package and the pd.read_html() function in python. This chunk of code like this one is usually good enough if there is only one table on the page.

![image](https://user-images.githubusercontent.com/112586829/197304812-9a76c504-efb7-4d1f-93a0-e754405398b2.png)

Note: The reason that I needed to assign RDTable to RDGrowths[0] is because pd_.read_html() will grab all the tables on the page and put them into a list of tables instead of a dataframe, even if there is only one table.  If there are mutiple tables, you can use a for loop and pd.concat to combine the dataframes.  Below I have shown an example since the webpage below has three tables.

![image](https://user-images.githubusercontent.com/112586829/197308439-6749bf51-2001-4a78-8c76-774e50dd6cd1.png)

### Part 2: Joining Data

While the data created by the above two code chunks is good, I want to increase the versatility of this data.  In addition to the growth rates of the characters, I think it's also important to include their class, origin game, and how many spaces they can move on the map.  Being able to analyze different units based on class is vital for the game design of Fire Emblem, since it is important to make sure that knights in heavy armor do not raise their Speed stat too much, or make their strength stat too low, as to break the immersion that the unit is an armored knight.

![image](https://user-images.githubusercontent.com/112586829/197306197-13cc3c4f-78f0-45b2-97e7-6a864a155a5b.png)

(As you can see, this knight has higher speed than she does defense!)

Luckily, serenesforests.net has datasets for this too.  This is one of the webpages that has tables with these variables, but I will also post a picture of what the dataframe looks like for reference.  https://serenesforest.net/radiant-dawn/characters/base-stats/

![image](https://user-images.githubusercontent.com/112586829/197306425-ff355bb4-efae-42e6-9fc1-3c96a0a829c0.png)

Since both tables have the exact same values for the name, I wanted to be sure to do an inner join where the 'Name' column was the index, since the Name columns all match.  I did this for each game's table.

Here's the code chunk and the resulting table.

![image](https://user-images.githubusercontent.com/112586829/197306911-e7ef46bc-aab7-4d18-b5ea-60a5e368f5c5.png)

(Note: RDBase is the table with the classes and movement data.  It was gathered the exact same way as the other tables, but with a different url)

This stackoverflow page is one of the best pages I've found explaining joins (or merge in Pandas).
https://stackoverflow.com/questions/5706437/whats-the-difference-between-inner-join-left-join-right-join-and-full-join

### Part 3: Cleaning the Data Before Final Assembly

Before merging the data into one ultimate dataframe, there was one last obstacle that I faced.  The data is not recorded the same for each game on serenesforest.net.  Compare the two dataframes.


![image](https://user-images.githubusercontent.com/112586829/197307385-282da390-678b-4d07-a89a-eb470e112b30.png)
![image](https://user-images.githubusercontent.com/112586829/197308625-19a2bb0c-17f3-41da-93b0-0837aea8b1d3.png)


The table above has Strength and Magic as separate stats while the one below has it as S/M.  This is because in some Fire Emblem games, units can't use both physical weapons and magic, so they either have only a strength or a magic stat.  I have added a visual to represent what I mean.


![image](https://user-images.githubusercontent.com/112586829/197307897-42a33a10-bbff-44be-be78-8370a3ef5a7c.png)
![197307989-08093f73-badb-487e-bfa0-5ce562fe3f31](https://user-images.githubusercontent.com/112586829/197308528-bdc9cdf5-f9b6-4751-969e-1fe5aeac3dc4.png)


To fix this, I created two new columns, ['Str'] and ['Mag'] and assigned them the values of the ['S/M'] column.  Then I made a for loop which set one or the other to np.nan depending on their class.  

![image](https://user-images.githubusercontent.com/112586829/197308162-b43a7e82-259a-44b5-89d4-5cf012d0bbd6.png)


### Final Assembly

With some final merge statements, we can get all of the data onto one table.  Here, we use an outer join, since we do want every row from each dataset.  

![image](https://user-images.githubusercontent.com/112586829/197308242-85f1d647-fa6e-444d-890d-fb07e4c2365e.png)

### Dataset Complete

Finally, we have all of the tables joined with an added Class, Mov, and Game Title variable.  If there is something you are interested in, I highly recommend creating your own dataset, even if one is already available.  It will exponentially increase the scope of what data you can analyze, but also set you apart from other data scientists, since you can also create datasets as well as explore them.

The code used to make this dataset can be found in my github repository here.
https://github.com/aldenm01/Blog_2_Code
