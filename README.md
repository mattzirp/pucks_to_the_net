# Pucks to the Net
An Analysis of Shots and Goals from NHL Game Data

This project was completed as the capstone for the University of Calgary course DATA601 - Working with Data and Visualization. Our team is made up of:

- Alec Lunn
- Dylan Medina
- Matthew Zirpoli

## Introduction
As students in a Canadian city, we are avid fans of hockey. As data science students, what better way to use our new skills than to dive into data from our favorite sport? In this project, we use data from [NHL Game Data](https://www.kaggle.com/datasets/martinellis/nhl-game-data/data) on Kaggle. The main goal of our project was to explore data about shots and goals, and elicit key insights about them. Specifically, we sought to answer the following guiding questions about our data:

1. Where on the ice do shots have the best percentage of scoring goals/Where are you most likely to score a goal? What types of shots are most likely to score?

2. How does the score differential affect the team's decision-making and strategy during the game?

3. What are the key differences in all-star calibre shooting versus league average shooting?

4. Have shot patterns changed over the years (ie do they succeed from different areas, do players favour different shot types/locations)?

## Dataset 
As mentioned, our dataset is [NHL Game Data](https://www.kaggle.com/datasets/martinellis/nhl-game-data/data), hosted on Kaggle.  The NHL Game Data dataset contains multiple tables stored in .csv format, and essentially represents a point in time retrieval of a relational database. The data set contains informational for all games played between the years of 2000 and 2020. 

Of particular interest is the game_plays table, which stores data regarding plays that happen in NHL games. Each row repesents a play in a game. Each recorded play has a type, and some have secondary types to further categorize the play. Recorded play types are things like goals, shots, faceoffs, penalties, hits, and other statistics driving events. The x,y coordinates for some of these play types, like shots and goals are also stored, as well as the players involved, the current game time, and the current score. This table offers plenty of opportunity to perform aggregations and answer many of our 

The games table stores information like, season, outcome, whether the game was settled in Regulation, Overtime, or a shootout, the score, etc. We will use this for time series analysis, as well as game situation analysis.

We can also walk across the game_plays_players table to the player_info table to get further information about players. This will allow us to identify players by name like Alex Ovechkin, rather than their ID number in the table. We use this identifying information to match up All star game data from [hockey-reference](https://www.hockey-reference.com/allstar/), using a player's selection for the all star game as a stand in for player quality. 

## Methods 

#### Data Cleaning


#### Baseline Analysis

#### Time Series Analysis

## Tools

### Data Cleaning and Analysis

- Jupyter Notebook
- Python
- NumPy
- pandas

### Visualization

- matplotlib
- seaborn
- sportypy

### Collaboration

- GitHub
