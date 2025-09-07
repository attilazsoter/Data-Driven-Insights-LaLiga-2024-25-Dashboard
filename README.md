# Data-Driven Insights from the 2024/25 La Liga Season

## Overview
This project analyzes the **2024/25 Spanish La Liga season** using match-level and player-level data to create an **interactive Tableau dashboard**.  
The primary goal was to provide actionable insights for scouts, coaches, and analysts in identifying **high-performing players** and **undervalued talents** across different teams.

The dashboard helps answer key questions such as:
- Which teams were most efficient in converting scoring opportunities?
- Which defenders stood out in terms of reliability and discipline?
- Which players could be considered undervalued gems despite lower rankings?

The project demonstrates the value of **data-driven scouting** by combining **statistical analysis** with **interactive visual exploration**.

---

## Data Sources and Data Preparation
- **Match Statistics (SP1):** [Football-Data.co.uk](https://www.football-data.co.uk/spainm.php)  
  - Includes results, goals, fouls, cards, shots on target, etc.  
- **Player Statistics (La_Liga_Players):** [FBref.com](https://fbref.com/)  
  - Includes minutes played, goals, assists, cards, penalties, and advanced per-90 metrics.

Both datasets were downloaded in **Excel**, then connected to **Tableau** for pre-processing and dashboard creation.

After I have removed the redundant columns in both the SP1 and La_Liga_Players tables in Tableau, had to join these tables through the Team (or Club) attribute Primary/Foreign key relationship. However, as we had 2 columns for the team in the SP1 table (Home Team and Away Team), I had to select the Home Team and Away Team columns in the SP1 database in Tableau and unpivot them into a single column called Team and it also added an additional Home/Away column. As a result, the table generated 2 rows for each game, one with the home team’s name in the Team column and one with the away team’s name in the Team column (and the measures got repeated in the 2 rows).

---

## Dashboard Design

### Visuals and Key Features
#### League Table
Includes team standings and this table also serves as a filter for some of the other visuals for interactive exploration.
For this visual I had to create a calculated measure in the SP1 (March Statistics) table for the number of points obtained during the season. For this I implemented a logic so that if the Full Time Result column showed H (i.e. home team win) and the Home/Away column showed “Home Team” then it granted 3 points to that team for the respective game. Similarly, if the Full Time Result column showed A (i.e. away team win) and the Home/Away column showed “Away Team” then it also granted 3 points. If the Full Time Result column showed D (Draw), it allocated 1 point, and in every other cases it allocated 0 points. Using the same logic (except that I used the Half Time Results column instead of the Full Time Result column), I also created a new calculated measure for Halftime Points, simulating what if all the games had ended at the end of the first half. This was another insightful statistic that I also added to the League table as it provided interesting insights into e.g. which teams were the worst and best finishers during the seasons.

As I also wanted to show the number of wins, draws and losses by team in this table, I also had to create calculated measures for them in the SP1 table. For example, in case of the new “Win” measure I created, if the Full Time Result column showed home team win and the Home/Away column showed Home Team or the Full Time Result column showed away team win and the Home/Away column showed Away Team it allocated a binary value of 1 to that row in the SP1 table, otherwise it allocated 0. I created the “Draw” and “Loss” measures following a similar logic.

As I also wanted to show the number of goals scored and conceded by team in this table, I also had to create calculated measures for them in the SP1 table. In case of the new “Goals For” measure I created, if the Home/Away column showed Home Team then it took the value from the Full Time Home Team Goals column, otherwise it took the value from the Full Time Away Team Goals column. I created the “Goals Against” measure following a similar logic.

After having created these measures, I added them aggregated by their sum into the columns of the League table and ranked the teams by number of points (in descending order). In addition, I also added a filter to this visual so that the user can analyze, how the teams ranked only looking at their home games or away games.

#### % of Goals per Shot Table
Ranks the teams (sorted in descending order, i.e. from best to worst) based on how effectively they converted their goal-scoring opportunities during the season. This gives the user some insights into the quality of their forwards.

In order to calculate the Goals Scored per Shot ratio for this visual, first I had to create a new calculated measure to show the number of shots attempted by team during the season: if the Home/Away column showed Home Team then it took the value from the Home Team Shots column, otherwise it took the value from the Away Team Shots column. And the Goals Scored per Shot measure was created by simply dividing the previously created Goals For measure by this Shots Attempted measure.

#### % of Goals Conceded per Shot on Target Received Table
Ranks the teams (sorted in ascending order) based of how efficient their goalkeeper was in catching the opponent’s shots.

In order to calculate the Goals Conceded per Shot on Target Faced ratio for this visual, first I had to create a new calculated measure to show the number of shots faced by team on their goal during the season: if the Home/Away column showed “Home Team” then it took the value from the Away Team Shots on Target column, otherwise it took the value from the Home Team Shots on Target column. And the Goals Conceded per Shot on Target Faced measure was created by simply dividing the previously created Goals Against measure by this Shots on Target Faced measure.

#### Top Goal Scorers and Top Assists Tables
These tables rank the players by number of goals scored and number of assists during the season (sorted in descending order).

#### Average Age of Players by Position Table
Bar chart sorting the teams in ascending order by age of their players. The users can select if they want this visual show defenders, midfielders, or forwards.

#### Number of Fouls / Yello Cards / Red Cards by Team Table
A treemap, which shows by team the number of fouls, yellow cards and red cards received during the season.

For the implementation of the selection (interactive) feature of this visual I first had to create a “Foul/Yellow/Red” parameter in Tableau to display the Fouls, Yellow Cards and Red Cards metrics in the dropdown. I also had to create calculated measures for number of fouls per team, number of yellow cards per team and number of red cards per team in a similar way to how I created the “Goals For” measure earlier (as an example). This means that in case of the new “Fouls (per team)” column, if the Home/Away column showed “Home Team” then it took the value from the Home Team Fouls Committed column, otherwise it took the value from the Away Team Fouls Committed column. The “Yellow Cards (per team)” and “Red Cards (per team)” columns were implemented using a similar logic. The last step for this visual was to implement another calculated metric called “Foul/Yellow/Red Selector”, which applied the Fouls (per team) measure to the visual in case the user selected Fouls in the drop down list in the “Foul/Yellow/Red” parameter, the Yellow Cards (per team) measure if the user selected Yellow Cards in the drop down list in the “Foul/Yellow/Red” parameter, and the Red Cards (per team) measure when the user selected Red Cards in the drop down list.

#### Defenders by Age and Minutes Played in the Season
A scatterplot that shows the defenders with the number of minutes played on the x-axis and the age on the y-axis and it also elaborates on the player’s name, club, number of yellow cards and number of red cards received during the season in the tooltip.

### Design Considerations
- Clear logical ordering of visuals.  
- Interactive filters to reduce clutter and allow deep dives.  
- Use of calculated measures (e.g., Goals per Shot, Cards per Match) to enrich analysis.  

---

## Results and Insights
- Identified several undervalued defenders from lower-ranked teams who played high minutes with low card counts.  
- Highlighted teams with strong defensive efficiency despite mid-table rankings.  
- Illustrated the potential of data-driven scouting in discovering hidden talent and avoiding bias toward only top-ranked clubs.

---

## Tools and Technologies
**Tableau**: Data cleaning and preparation, dashboard development and visualization
