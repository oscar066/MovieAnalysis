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

Importing and alias Pandas as pd
Import and alias matplotlib.pyplot as plt

Import and alias seaborn as sns

Set Matplotlib visualizations to display inline in the notebook

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

## Data Understanding

### Loading Data

bom_df = pd.read_csv("bom.movie_gross.csv",index_col=0)
bom_df.head(5)
Looking at the info printout
bom_df.info()
bom_df.isna().sum()
Title_df = pd.read_csv("imdb.title.basics.csv", index_col=0)
Title_df.head()
Title_df.info()
Title_df.isna().sum()
Ratings_df = pd.read_csv("imdb.title.ratings.csv.gz",index_col=0)
Ratings_df.head()
Ratings_df.info()
Ratings_df.isna().sum()

## Data Cleaning
### Identifying and Handling Missing Values

bom_df.info()
bom_df.dtypes

# Removing the comma in currency 

bom_df['foreign_gross'] = bom_df['foreign_gross'].str.replace(',','')
#Converting the foreign gross from object to float to allow calculations
bom_df = bom_df.astype({'foreign_gross':float})
bom_df['domestic_gross'].fillna(bom_df['domestic_gross'].median(), inplace=True)

bom_df['foreign_gross'].fillna(bom_df['foreign_gross'].median(), inplace=True)

bom_df['studio'].fillna('Missing', inplace=True)
#creating a new column WorldWide_gross for the summation of Domestic and Foreign gross
bom_df['WorldWide_gross'] = bom_df['domestic_gross'] + bom_df['foreign_gross']
#Making sure there are no NAN values after the cleaning
bom_df.isna().sum()
"""
Dealing with the numerical data by replacing the NAN values with their median value and for the categorical data replacing the NAN values with a descriptive word 'Missing'
"""
Title_df.info()
Title_df.isna().sum()
Title_df['original_title'].fillna('Missing',inplace=True)

Title_df['runtime_minutes'].fillna(Title_df['runtime_minutes'].median(), inplace=True)

Title_df['genres'].fillna('Missing', inplace=True)
#Making sure there are no NAN values after the cleaning
Title_df.isna().sum()
"""
Dealing with the numerical data by replacing the NAN values with their median value and for the categorical data replacing the NAN values with a descriptive word 'Missing'
"""
Ratings_df.isna().sum()

## Data Visualization

###  Question 1: What films are currently doing best at box office?

#Creating a function to group data by their mean 
def class_grouped_means(data,columns,items_to_group_by):
    grouped_data = data.groupby(items_to_group_by)[columns].mean()
    grouped_df = pd.DataFrame(grouped_data)
    grouped_data.reset_index( drop= False, inplace= True)

    return grouped_df
grouping = class_grouped_means(bom_df,['WorldWide_gross'],'title')
grouping_1 = grouping.sort_values(by=['WorldWide_gross'], ascending=False).head(10)
grouping_1.head()
#Visualisation
sns.set_style('darkgrid')

bar , ax = plt.subplots(figsize=(18,10))
ax = sns.barplot(x = 'WorldWide_gross', y = 'title',data= grouping_1 ,palette='GnBu_d',orient='h')

ax.set_title('Best Performing Films as per the WorldWide_gross',fontsize= 30)
ax.set_xlabel('Title of Best (10) Perfoming Movies',fontsize= 24)
ax.set_ylabel('Worldwide_Gross in $',fontsize= 24)

plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
bar.savefig("Title_VS_WorldWide_gross.png")


Analsis of the bar plot
"""
This shows the top 30 best performing movies currently at box office as per the WorldWide_Gross ($) they generated
"""
### Question 2: What Genres of films are currently doing best at box office?

#dropping the original_title column since it is not relevant to the analysis as we already have the primary_title column
Title_df.drop(columns='original_title',inplace=True)
#renaming the primary_title to title to make it easier to use the column for merging purposes
Title_df.rename(columns={'primary_title':'title'} ,inplace=True)
#merging the bom_df and Title_df on column title
Budget_Title_df = pd.merge(bom_df, Title_df, on='title')
#setting the index of Budget_Title_df to title
Budget_Title_df.set_index('title', inplace=True)
grouping_2 = class_grouped_means(Budget_Title_df,['WorldWide_gross'],'genres')
grouping_3 = grouping_2.sort_values(by=['WorldWide_gross'], ascending=False).head(10)
grouping_3.head()

#### I : In terms of Worldwide Gross income

#Visualisation
sns.set_style('darkgrid')

bar , ax = plt.subplots(figsize=(18,10))
ax = sns.barplot(y = 'genres', x = 'WorldWide_gross',data= grouping_3 ,palette='magma', orient='h')

ax.set_title('Best Performing Films as per the WorldWide_gross',fontsize= 30)
ax.set_ylabel('Best (10) Perfoming Genres',fontsize= 24)
ax.set_xlabel('Worldwide_Gross in $',fontsize= 24)

plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
bar.savefig("Genres_VS_WorldWide_gross.png")


Analysis of the visualisation
"""
Analysis of the top 30 performing genres in terms of the WorldWide_gross income they generated
"""
#merging Title_df and Ratings_df on the column tconst
Title_Rating_df = pd.merge(Title_df, Ratings_df, on='tconst')

#### II : In terms of Ratings

budget_Title_Ratings_df = pd.merge(Budget_Title_df, Title_Rating_df ,on='title' )
budget_Title_Ratings_df.drop(columns=['start_year_x','runtime_minutes_x','genres_x'],inplace=True)
budget_Title_Ratings_df.rename(columns={'start_year_y':'start_year','runtime_minutes_y':'runtime_minutes','genres_y':'genres'},inplace=True)
budget_Title_Ratings_df.head()
grouping_4 = class_grouped_means(budget_Title_Ratings_df,['averagerating'],'genres')
grouping_5 = grouping_4.sort_values(by=['averagerating'], ascending=False).head(10)
grouping_5.head()
sns.set_style('darkgrid')

bar , ax = plt.subplots(figsize=(18,10))
ax = sns.barplot(y = 'genres', x = 'averagerating',data= grouping_5 ,palette='copper',orient='h')

ax.set_title('Best Performing Films as per the Ratings',fontsize= 30)
ax.set_ylabel('Best (20) Perfoming Genres',fontsize= 24)
ax.set_xlabel('Ratings ',fontsize= 24)

plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
bar.savefig("Genres_VS_Ratings.png")


"""
Analysis of the top 20 performing genres in relation to their ratings
"""
grouping_6 = class_grouped_means(budget_Title_Ratings_df,['numvotes'],'genres')
grouping_7 = grouping_6.sort_values(by=['numvotes'], ascending=False).head(10)
grouping_7.head()
sns.set_style('darkgrid')

bar , ax = plt.subplots(figsize=(18,10))
ax = sns.barplot(y = 'genres', x = 'numvotes',data= grouping_7 ,palette='Greens')

ax.set_title('Best Performing Films as per the Num of votes',fontsize= 30)
ax.set_ylabel('Best (10) Perfoming Genres',fontsize= 24)
ax.set_xlabel('Number of votes',fontsize= 24)

plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
bar.savefig("Genres_VS_NumVotes.png")


### Question 3: What is the relationship between genres and foreign and domestic gross income?

#### I. Domestic gross vs genres

grouping_8 = class_grouped_means(budget_Title_Ratings_df,['domestic_gross'],'genres')
grouping_9 = grouping_8.sort_values(by=['domestic_gross'], ascending=False).head(10)
grouping_9.head()
sns.set_style('darkgrid')

bar , ax = plt.subplots(figsize=(18,8))
ax = sns.barplot(y = 'genres', x = 'domestic_gross',data= grouping_9 ,palette='Blues')

ax.set_title('Best Performing Films Genres as per the Domestic gross',fontsize= 30)
ax.set_ylabel('Best (10) Perfoming Genres',fontsize= 24)
ax.set_xlabel('Domestic gross ($)',fontsize= 24)

plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
bar.savefig("Genres_VS_Domestic_gross.png")


#### II. Foreign gross vs genres

grouping_10 = class_grouped_means(budget_Title_Ratings_df,['foreign_gross'],'genres')
grouping_11 = grouping_10.sort_values(by=['foreign_gross'], ascending=False).head(10)
grouping_11.head()
sns.set_style('darkgrid')

bar , ax = plt.subplots(figsize=(18,8))
ax = sns.barplot(y = 'genres', x = 'foreign_gross',data= grouping_11 ,palette='Blues')

ax.set_title('Best Performing film Genres as per the Foreign gross',fontsize= 30)
ax.set_ylabel('Best (10) Perfoming Genres',fontsize= 24)
ax.set_xlabel('Foreign gross ($)',fontsize= 24)

plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
bar.savefig("Genres_VS_foreign_gross.png")


### Question 4: What is the relationship between Worldwide gross income and Ratings

pearsoncorr = budget_Title_Ratings_df.corr(method='pearson')
pearsoncorr

"""
There is weak positive correlation between WorldWide gross and average rating of 0.059501	
"""
fig, ax = plt.subplots(figsize=(16, 10))

sns.regplot(
    x=budget_Title_Ratings_df["WorldWide_gross"],
    y=budget_Title_Ratings_df["averagerating"],
    
)

ax.set_xlabel("WorldWide_gross ($)",fontsize=18)
ax.set_ylabel("Average Ratings",fontsize=18)
ax.set_title("WorldWide_gross vs Ratings",fontsize=24);

plt.xticks(fontsize=16)
plt.yticks(fontsize=16)
plt.savefig("Scatter_WorldWide_gross_VS_Ratings.png")
"""
This shows a weak positive correlation as the average rating increases the WorldWide gross may increase or it may not increase and the trend line is slightly positive
"""
### Question 4: What is the relationship between number of votes and the average rating

pearsoncorr_2 = budget_Title_Ratings_df.corr(method='pearson')
pearsoncorr_2
"""
There is a weak positive correlation between the num of votes and the average rating of the movies of 0.242963
"""
fig, ax = plt.subplots(figsize=(15, 10))

sns.regplot(
    x=budget_Title_Ratings_df["numvotes"],
    y=budget_Title_Ratings_df["averagerating"],
    
)

ax.set_xlabel("Number of votes",fontsize=18)
ax.set_ylabel("Average Ratings",fontsize=18)
ax.set_title("Number of Votes vs Ratings",fontsize=24);

plt.xticks(fontsize=16)
plt.yticks(fontsize=16)
plt.savefig("Scatter_NumVotes_VS_Ratings.png")
"""
There is a positive correlation between Number of votes and the average ratings of the movie and the trend line is positive
"""
### Question 5: What is the relationship between the number of votes and the WorldWide gross?

pearsoncorr_3 = budget_Title_Ratings_df.corr(method='pearson')
pearsoncorr_3
"""
There is a positive correlation between the number of votes and the Worldwide gross of 0.569319
"""
fig, ax = plt.subplots(figsize=(15, 10))

sns.regplot(
    x=budget_Title_Ratings_df["WorldWide_gross"],
    y=budget_Title_Ratings_df["numvotes"],
    
)

ax.set_xlabel("WorldWide gross ($)",fontsize=18)
ax.set_ylabel("Num of votes",fontsize=18)
ax.set_title("Num of Votes Vs. WorldWide gross",fontsize=24);

plt.xticks(fontsize=16)
plt.yticks(fontsize=16)
plt.savefig("Scatter_NumVotes_VS_WorldWide_gross.png")
"""
There is a positive correlation between the num of votes and the WorldWide gross as the trend line is positive
"""
## Summary

While there are many other factors that we could consider in a future analysis we feel that the following conclusions will result in a successful business venture as Microsoft enters the movie industry.
