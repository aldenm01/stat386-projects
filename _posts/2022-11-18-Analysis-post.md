---
layout: post
title:  Analyzing Statistics for Game Design
date:   2022-11-18
author: Alden
description: Analyzing the data from my last blog post for my new game project
image: https://user-images.githubusercontent.com/112586829/197297566-24c3e4f4-a966-43b8-8207-bd3b93b6736a.png
---

Have you ever dreamed of designing your own video game?  For the average person, it may seem that the entirety of game development is writing its code and creating the assets.
However, there is a lot more to game design than this.  Learning how to properly balance games is an integral part of keeping them engaging.  Can you imagine playing a 
game of chess where every piece you had was a queen while your opponent could only use pawns?  That is a game does not give the satisfaction of overcoming any sort
of challenge that games aim to achieve.  I got to thinking, how could I use my statistics knowledge and prior knowledge of other games in the series to help predict how 
to design stats for my new role playing game inspired by Fire Emblem.  

I have explained Fire Emblem in detail in my other posts, so if you want a more in-depth explanation, you can read those here, but I will summarize briefly.  Fire Emblem 
is a tactical role playing game where you level up soldiers to make them stronger.  Each stat for each unit has a different percent chance to increase by one point after leveling up.
For example, units in the knight class have a higher percentage of increasing defense after leveling up when compared to a mage.

![image](https://user-images.githubusercontent.com/112586829/202888134-3ff2cffc-084a-4243-a999-aa8fca580c5d.png)
![image](https://user-images.githubusercontent.com/112586829/202888275-17d2dcb3-189a-4124-81cd-7170bc2317b9.png)

## The Explored Data
The dataset explored in this post is from my previous blog.  However, I added class types for each unit.  For example, if a unit is a cavalier, they are a cavalry unit.
If a unit is a knight, they are an armored unit.  The last column that was added was the amount of votes that each unit got in an official Fire Emblem popularity poll,
where the top two characters from a male and female division were to get special art made for them for Fire Emblem's mobile game.  The data can be found on this links.
I felt that the amount of votes that a character got would be a great variable to see if it would help visualize what makes a fun unit for the average player.

https://fireemblem.fandom.com/wiki/Fire_Emblem_Heroes/Choose_Your_Legends_Results:_Round_1

https://vote4.campaigns.fire-emblem-heroes.com/en-US/results

# Boxplot of Class Type and Character Votes (Outliers included)

![image](https://user-images.githubusercontent.com/112586829/202888397-780950c4-f172-4122-bd00-753f36e7a9c3.png)

This is a messy looking plot.  I was really disappointed seeing this as the outliers in this plot
skew the data so much that I did not feel that it was a good representation of the population.  However,
I did figure out why it was so skewed.

Main characters usually get a tremendous amount of votes compared to characters who don't.  While
many players of the series love these characters, I decided to perform the rest of the analysis
with units who have 6000 or less votes, as most of the data is still there.  I am just taking
out these enormous outliers.

Compare:

![image](https://user-images.githubusercontent.com/112586829/202889308-05758696-2050-430c-aee6-af6a1487e21d.png)

From the image, we can see there is a huge difference between the amount of votes even from first to second place.

# Revised Boxplot without Outliers

![image](https://user-images.githubusercontent.com/112586829/202888444-64f10987-11dd-457e-a3ee-fa0bcde8ea7c.png)

# Regression Plot for Infantry Units

![image](https://user-images.githubusercontent.com/112586829/202888910-4fad2e77-e989-4f54-8ed3-d0736321b13f.png)

![image](https://user-images.githubusercontent.com/112586829/202888451-7ff9996d-b899-43d4-acc1-4be9c40ff418.png)

# Correlation Chart for Each Class Type

![image](https://user-images.githubusercontent.com/112586829/202889405-947d8f2f-cba9-4963-a1e6-055d1116bd20.png)
