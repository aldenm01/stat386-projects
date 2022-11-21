---
layout: post
title:  Using Data Analysis in Video Game Development
date:   2022-11-18
author: Alden
description: Data exploration showcasing how statistics has helped me on my game development project.
image: https://user-images.githubusercontent.com/112586829/202991110-ced6018a-1717-4dfc-bb43-b49562f78387.png
---

Have you ever dreamed of designing your own video game?  For the average person, it may seem that the entirety of game development is writing its code and creating the assets.
However, there is a lot more to game design than this.  Learning how to properly balance games is an integral part of keeping them engaging.  Can you imagine playing a 
game of chess where every piece you had was a queen while your opponent could only use pawns?  That is a game does not give the satisfaction of overcoming any sort
of challenge that games aim to achieve.  I got to thinking, how could I use my statistics knowledge and prior knowledge of other games in the series to help predict how to design characters and their stats, as well as study what makes a character popular to players.

![202978436-dd4c4b06-378e-4d8e-b996-1781d33ee63e](https://user-images.githubusercontent.com/112586829/202990469-882280d0-6ea6-4cf4-83fd-0e548ff88030.png)

If you are unfamiliar with Fire Emblem, I will summarize it briefly.  Fire Emblem is a tactical role playing game where you level up soldiers to make them stronger.  Each stat for each unit has a different percent chance to increase by one point after leveling up.
For example, units in the knight class have a higher percentage of increasing defense after leveling up when compared to a mage.

![202888134-3ff2cffc-084a-4243-a999-aa8fca580c5d](https://user-images.githubusercontent.com/112586829/202999725-763f58c2-fea7-4e62-9345-85f891bb18e9.png)![202888275-17d2dcb3-189a-4124-81cd-7170bc2317b9](https://user-images.githubusercontent.com/112586829/202999656-82efcf30-d0bc-4b66-8583-c0f57e0be4ab.png)

I have explained Fire Emblem in detail in my other posts, so if you want a more in-depth explanation, you can read about it on my two other blogs below.

[Blog 1](https://aldenm01.github.io/stat386-projects/2022/09/26/Blog-Tutorial.html)
[Blog 2](https://aldenm01.github.io/stat386-projects/2022/09/26/Dataset-post.html)

## The Dataset
The dataset explored in this post is from my previous blog.  However, I added class types for each unit.  For example, if a unit is a cavalier, they are a cavalry unit.
If a unit is a knight, they are an armored unit.  The last column that was added was the amount of votes that each unit got in an official Fire Emblem popularity poll for the mobile game.  
I felt that the amount of votes that a character got would be a great variable to see if it would help visualize what makes a likeable unit for the average player.

Data from these polls were collected from the links below.

[2017 Poll](https://fireemblem.fandom.com/wiki/Fire_Emblem_Heroes/Choose_Your_Legends_Results:_Round_1)

[2019 Poll](https://vote4.campaigns.fire-emblem-heroes.com/en-US/results) (For data for newer characters)

# Question 1: Are certain classes more popular then others?

I decided to explore these questions with a boxplot.

## Boxplot of Class Type and Character Votes (Outliers included)

![image](https://user-images.githubusercontent.com/112586829/202979589-bd5123c6-25f5-4707-8ce8-a163f9ff8679.png)

This is not a great plot.  I was disappointed seeing this as the outliers in this plot
skew the data so much that it is not an accurate representation of the population.  However,
I did figure out why it was so skewed.

Main characters usually get a tremendous amount of votes compared to characters who don't.  While
many players of the series love these characters, I decided to perform the rest of the analysis
with units who have 6000 or less votes since 211 of the 264 data points are still there.

Compare:

![image](https://user-images.githubusercontent.com/112586829/202889308-05758696-2050-430c-aee6-af6a1487e21d.png)

From the image, we can see there is a huge difference between the amount of votes even from first to second place.

# Revised Boxplot Without Extreme Outliers

![image](https://user-images.githubusercontent.com/112586829/202989825-2d83ef75-9b4d-4786-af24-97f766307b55.png)

Here is a boxplot that gives a much better visual of the population.  We can see that infantry and flying units are popular, while armor units have an extremely low amount of votes.  A personal theory I have of why this is is because these units have much more unique and "cool" designs than armored units which I assume many players graviate to, while armored units may seem basic.

(**Infantry Units:**)

![image](https://user-images.githubusercontent.com/112586829/202975594-bb4afe9d-8435-4606-b107-3e10c81f99b1.png)  ![Screenshot 2022-11-20 230200](https://user-images.githubusercontent.com/112586829/202976856-4537696e-bd30-4ced-a889-665185f7aa37.png)  

Nephenee (Left): 5,557 Votes  
Masked Marth (Right): 5,420 Votes

(**Armored Units:**)

![image](https://user-images.githubusercontent.com/112586829/202975800-3f4570f8-0eba-4932-aff1-e35bf1dac494.png)  ![image](https://user-images.githubusercontent.com/112586829/202979167-179e76bb-8660-4cb3-8357-54a4d5408c80.png)


Barth: (Left): 117 Votes   
Oswin: (Right): 598 Votes

# Question 2: Are Strength and Defense Correlated?

![image](https://user-images.githubusercontent.com/112586829/202888910-4fad2e77-e989-4f54-8ed3-d0736321b13f.png) ![image](https://user-images.githubusercontent.com/112586829/202888451-7ff9996d-b899-43d4-acc1-4be9c40ff418.png)

I decided to make my first regression plot only include rows in the data where the unit is in an infantry class.  As we can see, strength and defense are positively correlated, and it is a slightly stronger correlation than when compared to all the data.  It seems that it is a good idea to make a unit's attack relative to its defense.

# Correlation Matrices for All Class Types:

![image](https://user-images.githubusercontent.com/112586829/202986524-a61b3070-9bcf-43d1-8cb3-16a1a02e6d2d.png) ![image](https://user-images.githubusercontent.com/112586829/202986552-23bfc29a-edd9-4c52-a36f-bf85246d1e2b.png) ![image](https://user-images.githubusercontent.com/112586829/202986563-1582eb1d-8533-4da3-abaf-516f6152a2a5.png) ![image](https://user-images.githubusercontent.com/112586829/202986577-d1ed3a15-3345-41b9-9e6b-8c73533d42d7.png) ![image](https://user-images.githubusercontent.com/112586829/202986603-9001522d-21e3-42c7-88a1-7702e234dcc0.png) ![image](https://user-images.githubusercontent.com/112586829/202986620-a6c5fab0-1f94-43db-9f7b-8d5ea023b107.png)

In these charts, we can see the correlation of each variable for each class.  An interesting observation from this plot is that flying units have a much lower correlation between their health points and strength.  One other point that is interesting is the lack of significance in the relationship betwewen stats, as there are many relationships in these charts that do not have a strong correlation.  While these charts may seem a bit more cluttered, they make a great reference when I am designing a unit of a certain class and need a quick refresher on these correlations.

A great article for explaining correlation matrix can be found [here](https://corporatefinanceinstitute.com/resources/excel/correlation-matrix/#:~:text=A%20correlation%20matrix%20is%20simply,patterns%20in%20the%20given%20data.)

# Summing up the Findings

From these charts, I now have a better understanding on how to design a tactical RPG like Fire Emblem.  Infantry and flying units are a favorite among players, so I will include several in my project while having few armored characters.  Additionally, strength and defense have a strong correlation with not just infantry units, but for all class types and the correlation matrices will be a great reference when designing units of all classes.  I'm glad that I was able to bridge my passion for data, coding, and games by analyzing my own unique dataset.  I encourage all of my readers to do the same with their passions, and leave a comment below on how it has affected your life!  I hope you bring much to report!

![image](https://user-images.githubusercontent.com/112586829/202995696-2d999235-cd59-4104-9322-1a9e6428df3d.png)

Code and data used for this analysis can be found in this [repository](https://github.com/aldenm01/Blog_3_Data).
