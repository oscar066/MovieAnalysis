#                          Project Movies Analysis
## Project overview

For this project, you will use exploratory data analysis to generate insights for a business stakeholder.


## Business Problem

Microsoft sees all the big companies creating original video content and they want to get in on the fun. They have decided to create a new movie studio, but they don’t know anything about creating movies. You are charged with exploring what types of films are currently doing the best at the box office. You must then translate those findings into actionable insights that the head of Microsoft's new movie studio can use to help decide what type of films to create.


## Objectives

1. Importing the required libraries

2. Loading Data from the files

3. Data understanding by analysing the data

4. Data Cleaning and handling of NAN values in the datasets

5. Data Visualisation 

## Sources of My Datasets

1. imdb.title.basics

2. imdb.title.ratings

3. bom.movie_gross

## Importing Libraries

. Importing and alias Pandas as pd

. Import and alias matplotlib.pyplot as plt

. Import and alias seaborn as sns

## Data Understanding

### Loading Data

<p>bom_df = pd.read_csv("bom.movie_gross.csv",index_col=0)</p>

<p>Title_df = pd.read_csv("imdb.title.basics.csv", index_col=0)</p>

<p>Ratings_df = pd.read_csv("imdb.title.ratings.csv.gz",index_col=0)</p>


## Data Cleaning

<p> Identifying and handling NAN values </p>

<p> Replacing NAN values with median() for the numerical data </p>

<p> Replacing NAN values with a descriptive word 'Missing' </p>

## Data Visualization

###  Question 1: What films are currently doing best at box office?


<img src="Title_VS_WorldWide_gross.png" alt="Title_VS_WorldWide_gross">


### Question 2: What Genres of films are currently doing best at box office?

#### I : In terms of Worldwide Gross income


<img src="Genres_VS_WorldWide_gross.png" alt="Genres_VS_WorldWide_gross">


#### II : In terms of Ratings


<img src="Genres_VS_Ratings.png" alt="Genres_VS_Ratings">

<p>Analysis of the top 20 performing genres in relation to their ratings</p>


<img src="Genres_VS_NumVotes.png" alt="Genres_VS_NumVotes">

### Question 3: What is the relationship between genres and foreign and domestic gross income?

#### I. Domestic gross vs genres



<img src="Genres_VS_Domestic_gross.png" alt="Genres_VS_Domestic_gross">


#### II. Foreign gross vs genres


<img src="Genres_VS_foreign_gross.png">


### Question 4: What is the relationship between Worldwide gross income and Ratings

<p>pearsoncorr = budget_Title_Ratings_df.corr(method='pearson')</p>

<p>There is weak positive correlation between WorldWide gross and average rating of 0.059501</p>	



<img src="Scatter_WorldWide_gross_VS_Ratings.png">

<p>This shows a weak positive correlation as the average rating increases the WorldWide gross may increase or it may not increase and the trend line is slightly positive</p>

### Question 5: What is the relationship between number of votes and the average rating

<p>pearsoncorr_2 = budget_Title_Ratings_df.corr(method='pearson')</p>


<p>There is a weak positive correlation between the num of votes and the average rating of the movies of 0.242963</p>



<img src="Scatter_NumVotes_VS_Ratings.png">

<p>There is a positive correlation between Number of votes and the average ratings of the movie and the trend line is positive</p>

### Question 6: What is the relationship between the number of votes and the WorldWide gross?

<p>earsoncorr_3 = budget_Title_Ratings_df.corr(method='pearson')</p>


<p>There is a positive correlation between the number of votes and the Worldwide gross of 0.569319</p>


<img src="Scatter_NumVotes_VS_WorldWide_gross.png">

<p>There is a positive correlation between the num of votes and the WorldWide gross as the trend line is positive</p>

## Summary

While there are many other factors that we could consider in a future analysis we feel that the following conclusions will result in a successful business venture as Microsoft enters the movie industry.
