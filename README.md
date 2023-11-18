# League of Legends Neutral Objective Analysis, 2022 Data
Analysis of 2022 Competitive League of Legend Matches concerning Neutral Objectives by Annie Pham.
## Introduction
The following analysis has been done on data from 2022 Competitive League of Legends Matches which can be found at [oracleselixir-league-data](https://oracleselixir.com/tools/downloads). Each game of League of Legends has 12 corresponding rows, 6 per team with 5 of those 6 corresponding to a player on a team and the sixth row corresponding to the team's overall stats for the game. There are 123 columns in the dataset and almost 150,000 rows.  
The purpose of this analysis is to "find the relationship between teams claiming neutral objectives and their ability to win the game". The purpose of a game is to destroy the enemy team's base before they destroy yours, in the mean time however, there are a number of neutral objectives in the form of monsters that are available for either of the teams to take. These monsters include dragons, elder dragon, rift heralds, and barons, which not only provide gold to help strengthen members of the team, but also unique buffs.  
Many players, both competitive and casual, associate the claiming of these neutral objectives with winning the game, but the purpose of this analysis is to explore just how fundamental these objectives are to winning the game.  
With that being said, the columns relevant to this analysis are as follows: <mark>'gameid'</mark>, <mark>'league'</mark>, <mark>'side'</mark>, <mark>'playerid'</mark>, <mark>'teamname'</mark>, <mark>'teamid'</mark>, <mark>'result'</mark>, <mark>'dragons'</mark>, <mark>'opp_dragons'</mark>, <mark>'elders'</mark>, <mark>'opp_elders'</mark>, <mark>'heralds'</mark>, <mark>'opp_heralds'</mark>, <mark>'barons'</mark>, and <mark>'opp_barons'</mark>. These columns were chosen to help identify the matches that took place, the result of the matches, and the neutral objectives taken by teams participating in those matches. <mark>'playerid'</mark> was used to isolate team summaries as the data in the <mark>'playerid'</mark> column would be missing and then dropped. I additionally added 6 columns, <mark>'Total_Obj'</mark>, <mark>'percent_obj'</mark>, <mark>'more_drags'</mark>, <mark>'more_heralds'</mark>, <mark>'more_elders'</mark>, and <mark>'more_barons'</mark>.  
 The resulting dataframe has more than 20,000 rows and exactly 20 columns.
### Description of Columns
- <mark>'gameid'</mark> Unique id for each game
- <mark>'league'</mark> League in which the game took place, with leagues being uniquely named depending on their region and nationality. 
- <mark>'side'</mark> The side of which the team was playing, either 'red' or 'blue'
- <mark>'teamname'</mark> Name of the team playing.
- <mark>'teamid'</mark> Id of the team playing.
- <mark>'result'</mark> True or False, depending on whether the team won (True) or lost (False).
- <mark>'dragons'</mark>: Number of dragons taken by the team.
- <mark>'opp_dragons'</mark>: Number of dragons taken by the opponent.
- <mark>'elders'</mark>: Number of elders taken by the team.
- <mark>'opp_elders'</mark>: Number of elders taken by the opponent.
- <mark>'heralds'</mark>: Number of heralds taken by the team.
- <mark>'opp_heralds'</mark>: Number of heralds taken by the opponent.
- <mark>'barons'</mark>: Number of barons taken by the team.
- <mark>'opp_barons'</mark>: Number of barons taken by the opponent.
- <mark>'Total_Obj'</mark>: Number of neutral objectives (dragons, elders, heralds, and barons) taken in the game total.
- <mark>'percent_obj'</mark>: Percent of neutral objectives taken by that team
- <mark>'more_drags'</mark>: True or False, depending on whether the team had more dragons (True) or not (False).
- <mark>'more_heralds'</mark>: True or False, depending on whether the team had more heralds (True) or not (False).
- <mark>'more_elders'</mark>: True or False, depending on whether the team had more elders (True) or not (False).
- <mark>'more_barons'</mark>: True or False, depending on whether the team had more barons (True) or not (False).  
## Data Cleaning and Exploratory Data Analysis  
In cleaning the data, I first used a column called 'datacompleteness' in order to only look at complete data as I thought they would be the more reliable form of data I could use. I also used 'playerid' in the way I mentioned above, locating where 'playerid' was missing to grab only the team summary data. This meant I had two rows per game, one for each team that took part in that game. The 'result' column was also converted to a boolean. To calculate the 'Total_Obj' column I grouped by 'gameid' and summed the 'dragons', 'elders', 'heralds', and 'barons' columns together. To calculate percent_obj, I summed the 'dragons', 'elders', 'heralds', and 'barons' columns together for each team and then divided by the 'Total_Obj' I calculated earlier. Finally, I used a boolean comparison to calculate whether each team had more dragons, elders, heralds, and barons the opposing team by comparing 'dragons' to 'opp_dragons', 'elders' to 'opp_elders', and so on and so forth for heralds and barons. The last step I took to clean the data was filling in the missing values in the 'teamnname' column to equal 'unknown team' as there were already values of 'unknown team' present in the dataframe, so I wanted to make sure the missing values were consistent within that column.  
**Here is the head of the cleaned dataframe:**  

| gameid                | league   | side   | teamname                      | teamid                                  | result   |   dragons |   opp_dragons |   elders |   opp_elders |   heralds |   opp_heralds |   barons |   opp_barons |   Total_Obj |   percent_obj | more_drags   | more_heralds   | more_elders   | more_barons   |
|:----------------------|:---------|:-------|:------------------------------|:----------------------------------------|:---------|----------:|--------------:|---------:|-------------:|----------:|--------------:|---------:|-------------:|------------:|--------------:|:-------------|:---------------|:--------------|:--------------|
| ESPORTSTMNT01_2690210 | LCKC     | Blue   | Fredit BRION Challengers      | oe:team:68911b3329146587617ab2973106e23 | False    |         1 |             3 |        0 |            0 |         2 |             0 |        0 |            0 |           6 |       50      | False        | True           | False         | False         |
| ESPORTSTMNT01_2690210 | LCKC     | Red    | Nongshim RedForce Challengers | oe:team:d2dc3681437e2beb2bb4742477108ff | True     |         3 |             1 |        0 |            0 |         0 |             2 |        0 |            0 |           6 |       50      | True         | False          | False         | False         |
| ESPORTSTMNT01_2690219 | LCKC     | Blue   | T1 Challengers                | oe:team:6dcacec00a6ba7576c5ab7f30c995cd | False    |         1 |             4 |        0 |            0 |         1 |             1 |        0 |            2 |           9 |       22.2222 | False        | False          | False         | False         |
| ESPORTSTMNT01_2690219 | LCKC     | Red    | Liiv SANDBOX Challengers      | oe:team:5380cdbc2ad2b8082624f48f99f6672 | True     |         4 |             1 |        0 |            0 |         1 |             1 |        2 |            0 |           9 |       77.7778 | True         | False          | False         | True          |
| ESPORTSTMNT01_2690227 | LCKC     | Blue   | KT Rolster Challengers        | oe:team:b9733b8e8aa341319bbaf1035198a28 | True     |         4 |             1 |        0 |            0 |         1 |             1 |        1 |            0 |           8 |       75      | True         | False          | False         | True          |

### Univariate Analysis
<iframe src="assets/neutral-objectives-per-game.html" width=800 height=600 frameBorder=0></iframe>