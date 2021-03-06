# Technical Report
## Problem Statement

Every NBA team, fan, and analyst wants to know how well a players will play together. Since the answer to the question is only actionable when looking forward, the first step is to understand how well a player will do in the future. Player performance is too often boiled down to a single number (RPM, BPM, PER, etc.) and this would not provide a step towards understanding how well players would play as teammates nor does it recognize the complex reality of player performance. In this project, I look to predict player performance and tendencies in different aspects of the game.  

Carefully choosing these aspects was the first step in creating useful predictions. Looking through the vasts amount of data provided by the [NBA's website](https://www.NBA.com/stats), I determined four categories to use: catch-and-shoot plays, pull-ups, drives, and post-ups. By including assist, turnover, and shooting data, I calculated the expected value, in points, and rate, in possessions used per minute, for each player over the last 5 seasons (what was available).  

## Data Collection
Data collection occurred throughout this project as I recognized the need for more and more statistics. Most statistics were taken from individual player data from the [NBA's website](https://www.NBA.com/stats) and some data required to calculate expected values of assists each year was taken from [Basketball-Reference](https://www.basketball-reference.com).  

I have stored all of the data in the [data folder](./data) in this repository. You can also look through the [scraping notebook](./Data%20Gathering.ipynb) to see what data sets I acquired them and how I did so.  

## Exploratory Data Analysis
#### Determining Target Features
The early data analysis was all meant to understand the possible targets for the model. There were 8 possible categories that I looked into: paint touches, elbow touches, post-ups, drives, catch-and-shoot plays, and pull-ups. Unfortunately, there was some overlap among these categories, and this project was to understand the different dimensions of a player. Having overlapping categories would potentially lead to overrating and/or underrating certain players. So, I went with as many categories as I could that were clearly not overlapping: post-ups, drives, catch-and-shoot plays, and pull-ups.

The next step was to determine how to value the outcome of these possessions. The shooting categories were pretty straightforward as shots always end up in a shot. Effective field goal percentage (EFG%) is the expected points earned from a shot divided by the number of shots and then divided by two to make it comparable to field goal percentage. By multiplying it by two we have the expected value of a pull-up shot or catch-and-shoot shot. Drives and post-ups were harder since only some of the times they end a possession and scoring, turnovers, and assists are all methods to ending the possession.

To calculate them, I needed to value shots, assists, and turnovers. I used points percentage to value shots which includes free throws as well as field goals. For assists, some creativity was required. There are no available stats on the number of passes a player makes from post-ups or drives that turn into shots and the value of those shots that do go in. I used the league average points per assisted shot for each year and a player's assist to potential assist ratio to determine the value of an assist. Turnovers are a little more simple. I used league average points off turnovers for each year to value them. The formulas are below.  
<img src="./images/EV_AST.png" alt="Expected Value Assist Formula" title="Expected Value Assist Formula" height="100" />  
<img src="./images/EV_TO.png" alt="Expected Value Turnover Formula" title="Expected Value Turnover Formula" height="75" />


#### Valuing Target Features

The next step was determining the relationship of these target features with team performance. While this was not essential for the predictive modeling, it was necessary to interpreting the results. Looking at the graph below, you can see the correlation of the target features with DRTG (points allowed per 100 possessions), ORTG (points scored per 100 possessions), and win percentage. When interpreting the correlations from the table below, it is important to remember that each player is one fifth of the team.

![Correlation Between Target Features and Team Performance](./images/category_rating_correlation.png)

All expected value categories were correlated with better offensive ratings and were significantly less correlated with defensive ratings. It will be important to remember that these stats are unrelated to defensive performance. Similarly, and likely because they are not very related to defense, they are not closely related to win percentage, although they do have positive correlation with it. Also, rates were much less correlated with ORTG and win percentage. Therefore, knowing how often a player does something should not be looked at on its own, but in combination with how well they do it.

Another important observation is that post-up expected value was the only category to have a negative correlation with DRTG (which is good). This could be because posting-up is related to other information like height which might influence rebounding and defense.


#### Expected Values

Post-ups provided the greatest challenge since most players rarely or never do it. Still, the number of usable and meaningful observations were small for all categories. It was important to establish a cut-off to limit the noise in the data, but this needed to be balanced with creating too small of a sample size. Using a cutoff of 50 plays a year, I found the most correlated features with each category.

For post-ups, you can see that the most correlated categories are all related to how well a player shoots, passes, and avoids turnovers.

![Graphs of Features Correlated to Post-Up Effective Value](./images/post_touch_correlations.png)

For drives, methods of scoring were correlated including pull-ups which is another method of scoring off the dribble.

![Graphs of Features Correlated to Post-Up Effective Value](./images/drive_ev_correlations.png)

#### Rates

## Modeling
#### Expected Value
Using the features determined in the EDA, I applied regularized regression methods. This created easy interpretation. The R2 score depended on the category, but ranged between .15 to .78. The predictions were all compared to a baseline of the past season stats as an estimation of the next season. The model's improved the baseline from between 4 and 27%. Below is a graph showing the most important features and their coefficients.

![Graph with Post-Up Coefficients](./images/post_up_coefficients.png)

#### Rates

## Next Steps
Incorporating team data, to looking at how players work together.
