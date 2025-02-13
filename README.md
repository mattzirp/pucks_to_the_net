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

The image below shows a glance of the dataset tables and columns we will make use of, as well as how they are mapped to one another. 

![Dataset table map](/reports/figures/dataset_map.png)

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


## Methods and Results

### Data Cleaning
Our data comes to us in a .csv format that is mostly clean and free of missing data, particularly in our columns of interest. The key issues we note are a lack of data before a certain time period and duplication of rows. The league has updated its collection of statistics over time, and prior to the 2011 season, shots and shot types were only recorded for shots that resulted in goals. This can be fixed by filtering out those years which may skew data when we do time series analysis. For duplication, we address this with `pandas` built in `drop_duplicates` function, targetting duplication in columns which should serve as primary keys, and keeping only the such first occurence. 

### Baseline Analysis
We want to answer basic questions about shots and goals. What types of shots are most successful in scoring goals? Where are shots taken? Where are goals scored from? 

To answer questions about shot types, we can simply group and aggregate our data using `pandas`. The plot below shows us the success rate of different shot types

![Accuracy by Shot Type Bar Chart](/reports/figures/shot_accuracy_bar.png)

To answer questions about locations, we can take several approaches. First we take a purely mathematical approach by using distance and angle calculations to determine how far the shooter is and what angle off center they are for shots. We perform this using `pandas` to calculate new columns using the formulas:

distance:
$$D = \sqrt {(x - 89)^2 + (y - 0)^2}$$ 

angle:
$$\theta = \tan^{-1} ({\frac {y}{89 - x}}) * \frac {180}{\pi}$$

We also 'flip' the coordinates of each shot and goal to make it as though they all occur on the same side of the ice. Since we are not studying in the context of individual games, but rather league aggregate behavior over time, it makes our plots simpler to consider things as if they occurred within the same space, rather than producing two symmetrical distribtions. Calculating this correction on coordinates is simple, we flip the x coordinate over center ice by negating it, then to account for the different perspective, we multiply the y coordinate by the sign of the x coordinate.

Next we can visualize the distribution of shot distances and angles for shots and goals to see the common behaviors in shooting and how these effect scoring. The plot below shows the distributions of distance and shot angle for goals. We see that the closer to center and the closer to goal a player is, the more likely they are to score. 

![Shot Distance Distribution for Goals](/reports/figures/goal_distance_histogram.png)

![Shot Angle Distribution for Goals](/reports/figures/shot_angle_goal_histogram.png)

A better visual approach, however may be to show *density* of shots overlaid with a visual of a hockey rink. We use an open source package, sportpy to plot the ice surface underneath a heatmap to show us where shots and goals are most common. We can repeat this analysis for each shot type, showing us where shot types tend to come from, and succeed from, as well. Below, we see a heatmap visualization for all shot types, then a multiplot showing the heatmap for each individual shot type. This better contextualizes the same info we saw in the distance and angles to the actual game that we are considering!

![Shot Location Density Heatmap](/reports/figures/shot_location_heatmap.png)

### Time Series Analysis
We want to answer questions related to how shooting and goal scoring might vary over time. We approach this from two mindsets, changes over time within a game, and changes over the course of many seasons. 

To calculate in game time, we are given a period of play and the number of seconds elapsed in the current period. We know that there are 3, 20 minute periods in a hockey game, so we can calculate $((P - 1) * 1200) + \text{period time elapsed}$, where P is the current period of play. Once we have the total time elapsed in the game, we can visualize when shooting and scoring occurs throughout the minutes of a game by binnin each minute and plotting in a histogram

To study trends over seasons, we merge our season data from the games table, then using pandas grouping and aggregation functions to identify trends. Here, we must filter our shots data as the amount collected prior to 2011 for shots, and shot type is insufficient. The plots below show upward trends in

![Total Shots per Season (2011-2020)](/reports/figures/shots_by_season_trend.png)

![Goals per Season (2001 - 2020)](/reports/figures/goals_by_season_trend.png)


We also found that players have recently started to favor shooting wrist shots even more than usual. The plot below shows the percentage of total shots taken for each shot type. We note downward trends for slap shots, and upward trends for wrist shots.

![Percentage of Shots by Shot Type (2011-2020)](/reports/figures/percentage_of_shots_by_shot_type_season.png)


We again repeat our heatmap visualization of the location density over seasons, to show where shots are most often coming from when they result in goals. The below multiplot shows a heatmap for each season from 2011 to 2020. We note that there is no major trend in where goals are scored from, as always it's about getting pucks to the net!

![Goal Location Density by Season (2011 - 2020)](/reports/figures/goal_density_by_season.png)


### Game Situation Analysis

### All-Star vs. League-Average Analysis

In this part of the analysis, we wanted to look if there were any distinguishable key differences in All-Star player scoring versus League Average. Using the hockey-reference all-star data from the 2010-2020 seasons, we gathered the most frequent all-star players to visualize scoring patterns, goal/shot heatmaps, and K-Means clustering on goals. On the other side, we defined League-Average players as players aged 20-30 in the 2010-2020 seasons, fitting between the mean and one standard deviation of points scored for both offense and defensive positions. From there, we plotted the same visualizations as the All-Stars to see if there were any key differences between the two. 

Below is the distribution for League-Average Offense and Defense Players in terms of points
![Offensive Production Distribution](/reports/figures/offensive_production_distribution.png)

![Defensive Production Distribution](/reports/figures/defensive_production_distribution.png)

We first analysed the heatmap of goals from each group of players, seeing if there are any key differences in point production locations using the Seaborn heatmap visualization

![League-Average Goal Heatmap](/reports/figures/league_average_offenseman_goal_heatmap.png)

![All-Star Goal Heatmap](/reports/figures/all_star_shot_goal_heatmap.png)

We see there is quite a difference in some players veruss one another, we see that generally all-star players have a more defined goal production location on the ice, versus league average having production from everywhere, meaning they are taking less set up shots and shoot the puck from wherever they can. 

Next, we can see the difference in goals scored by period

![All-Star Goal Scoring by Period](/reports/figures/all_star_goal_scoring_by_period.png)

![League-Average Goal Scoring by Period](/reports/figures/league_average_goals_by_period.png)

From this we can see the two groups have the opposite of goal scoring from one another. Where League-Average is scoring mostly in the first period, and every period after the goal totals fall off. The exact opposite is seen with the All-Stars, as they score the most goals in the second and third period. 

Lastly, we will look at the K-Means clustering for both groups, seeing where the main locations are for goals scored across the group totals, instead of individual players. 

![K-Means Clustering for Goals Scored for All-Stars](/reports/figures/all_star_goal_clustering.png)

![K-Means Clustering for Goals Scored for League-Average](/reports/figures/league_average_goal_clustering.png)

The centroid locations for both groups are quite different from one another. A key difference is the third centroid being in the own teams zone for All-Stars, this points to all of the empty net goals scored by All-Star players in the last minutes of the game. Meaning that they are going to be on the ice more versus the league-average players. 

Overall, we see that there are many key differences between the two groups, with more analysis done in the final report, we see that all-stars should be considered all-stars versus league-average players, and that there are a wide range of skill and discrepances between the two. 

## Conclusions

By studying this NHL Data set and diving deep into shots and goals, we learned a lot about strategy and scoring at the highest level of hockey. Players prefer wrist shots by far, and they account for about 50% of all the shots taken. In spite of this preference, Deflections and Tip-ins are the shots with the highest success rate. The farther a player is away from the goal, and the farther away from the center of the ice in the y-direction, the less likely they are to score a goal. While players shoot from all over the offensive zone, they score the most from nearest the net. This may pose the question why players don't just exclusively shoot from 

In inspecting time-series analyses for trends. Looking within games, we found that more shots are taken and goals scored in the second period of games. This is possibly explained by the "long change", meaning that defensive players are further from their bench to make substitutions, and thus get stuck defending for long periods of time without a break. We also found that more goals are scored at the ends of periods, with a large standout being the third period. This is of course, due to the empty net goals that are scored when one team is down and pulls their goalie in favor of playing 6 v 5, hoping to even the score. When studying trends between seasons, we found that more shots are being taken and more goals are being score, suggesting a higher rate of play. We also note a transition to taking even more wrist shots, while taking less slap shots. We do not note any key trends in where shots are being taken from over time, however.


### Future Work
Using the analysis we have performed here, we anticipate there would be several applications for the knowledge developed. Visualizations like the shot charts produced for certain players and teams could be produced for coaches and inform their team strategy. With the insights about success of shots, we could also build a prediction model that tells us the likelihood of scoring a goal, given shot type, location, and player parameters. This might be useful in a coaching or gambling context, where simulations are performed to predict scores. Coaches could tune parameters to predict the effect on their chances of winning.


## References
1. Ellis, M. (2019). NHL Game Data [Online]. Available at:
https://www.kaggle.com/datasets/martinellis/nhl-game-data/data 

2. Hockey Reference (2024). NHL All-Star Game History & Statistics [Online]. Available at:
https://www.hockey-reference.com/allstar/ 

3. Python Software Foundation, 2023. Python (Version 3.11). Available at: https://www.python.org

4. Harris, C.R. et al., 2020. Array programming with NumPy. Nature, 585, pp.357–362. Available at: https://doi.org/10.1038/s41586-020-2649-2 

5. McKinney, W. & others, 2010. Data structures for statistical computing in Python. In Proceedings of the 9th Python in Science Conference, pp. 51–56. Available at: https://conference.scipy.org/proceedings/2010/mckinney.html 

6. Hunter, J.D., 2007. Matplotlib: A 2D graphics environment. Computing in Science & Engineering, 9(3), pp.90–95. Available at: https://doi.org/10.1109/MCSE.2007.55 

7. Waskom, M.L., 2021. Seaborn: statistical data visualization. Journal of Open Source Software, 6(60), pp.3021. Available at: https://doi.org/10.21105/joss.03021

8. Drucker, Ross, 2022. Sportpy: Python package for drawing regulation playing surfaces for several sports. Available at https://sportypy.sportsdataverse.org/index.html
