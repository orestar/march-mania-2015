
This code book describes :

#1/ Data Files
| File Name | Available Formats |  
| :-------: | :---------------: |  
| tourney_compact_results.csv | (56.27 kb) |  
| seasons.csv | (1.39 kb) |  
| teams.csv | (5.82 kb) |  
| tourney_detailed_results.csv | (76.51 kb)|  
| tourney_slots.csv | (37.42 kb) |  
| tourney_seeds.csv | (28.57 kb) |  
| regular_season_detailed_results.csv | (5.76 mb) |  
| regular_season_compact_results.csv | (3.76 mb) |  
| sample_submission.csv | (177.98 kb) |  
| own sources of data | (000 kb) |  

#2/ Variables

#3/ Transformations 

# Context :

The data description and schema may seem daunting at first, but is not as complicated as it appears.  
As a reminder, you are encouraged to **incorporate your own sources of data**.  
We have provided team-level historical data to jump-start the modeling process, but there is also player-level and game-level data that may be useful.  
If you want, you could load your data into the Vertica Analytics Platform 

# What to predict

> - **Stage 1** - You should submit predicted probabilities for every possible matchup in the past 4 NCAA tournaments (2011-2014).
 
> - **Stage 2** - You should submit predicted probabilities for every possible matchup before the 2015 tournament begins.

Refer to the Timeline page for specific dates.  
In both stages, the sample submission will tell you which games to predict.

# File descriptions:    
Below we describe the format and fields of the "essential" data files.  
Optional files may be added to the data while the competition is running.  
You can assume that we will provide the essential files for the current season.  
You should not assume that we will provide optional files for the current season.  
To avoid confusion, we will keep the current season data (for **stage 2**) separate from the historical data (**stage 1**).
 
# teams.csv  
This file identifies the different college teams present in the dataset.
Each team has a 4 digit id number.  
# seasons.csv  
This file identifies the different seasons included in the historical data, along with certain season-level properties.  
 •"season" - indicates the year in which the tournament was played  
 •"dayzero" - tells you the date corresponding to daynum=0 during that season.  
All game dates have been aligned upon a common scale so that the championship game of the final tournament is on daynum=154.  
Working backward,  
**national semifinals** are always on **daynum=152**,  
**"play-in" games** are on days **134/135**,  
**Selection Sunday** is on day **132**, and so on.  
All game data includes the day number in order to make it easier to perform date calculations.  
If you really want to know the exact date a game was played on, you can combine the game's **"daynum"** with the season's **"dayzero"**.  
For instance, since day zero during the 2011-2012 season was **10/31/2011**, if we know that the earliest regular season games that year were played on **daynum=7**, they were therefore played on **11/07/2011**.  
 •"region W/X/Y/Z" - by convention, the four regions in the final tournament are always named **W**, **X**, **Y**, and **Z**.
Whichever region's name comes first alphabetically, that region will be Region W.  
And whichever Region plays against Region **W** in the **national semifinals**, that will be Region **X**.  
For the other two regions, whichever region's name comes first alphabetically, that region will be Region **Y**, and the other will be Region **Z**.  
This allows us to identify the regions and brackets in a standardized way in other files.  
For instance, during the 2012 tournament, the four regions were East, Midwest, South, and West.  
Being the first alphabetically, **East** becomes **W**.  
Since the **East** regional champion (Ohio State) played against the **Midwest** regional champion (Kansas) in the national semifinals, that makes **Midwest** be region **X**.  
For the other two (South and West), since **South** comes first alphabetically, that makes **South Y** and therefore **West** is **Z**.  
So for this season, the **W/X/Y/Z are East,Midwest,South,West**.


# regular_season_compact_results.csv  
This file identifies the game-by-game results for 30 seasons of historical data, from **1985 to 2014**.  
Each year, it includes all games played from daynum 0 through 132 (which by definition is "Selection Sunday", the day that tournament pairings are announced).  
Each row in the file represents a single game played.  
"**season**" - this is the year of the associated entry in **seasons.csv** (the year in which the final tournament occurs)  
"**daynum**" - this integer always ranges from 0 to 132, and tells you what day the game was played on.  
It represents an offset from the "dayzero" date in the "seasons.csv" file.  
For example, the first game in the file was daynum=20.  
Combined with the fact from the "season.csv" file that day zero was 10/29/1984, that means the first game was played 20 days later, or 11/18/1984.  
There are no teams that ever played more than one game on a given date, so you can use this fact if you need a unique key.  
In order to accomplish this uniqueness, we had to adjust one game's date.  
In March 2008, the SEC postseason tournament had to reschedule one game (Georgia-Kentucky) to a subsequent day, so Georgia had to actually play two games on the same day.  
In order to enforce this uniqueness, we moved the game date for the Georgia-Kentucky game back to its original date.  
**wteam** - this identifies the id number of the team that won the game, as listed in the "teams.csv" file.  
No matter whether the game was won by the home team or visiting team, "wteam" always identifies the winning team.  
**wscore** - this identifies the number of points scored by the winning team.  
**lteam** - this identifies the id number of the team that lost the game.  
**lscore** - this identifies the number of points scored by the losing team.  
**numot** - this indicates the number of overtime periods in the game, an integer 0 or higher.  
**wloc** - this identifies the "location" of the winning team.  
If the winning team was the home team, this value will be **H**.  
If the winning team was the visiting team, this value will be **A**.  
If it was played on a neutral court, then this value will be **N**.  
Sometimes it is unclear whether the site should be considered neutral, since it is near one team's home court, or even on their court during a tournament, but for this determination we have simply used the Kenneth Massey data in its current state, where the "@" sign is either listed with the winning team, the losing team, or neither team.  
 
# regular_season_detailed_results.csv
This file is a more detailed set of game results, covering seasons 2003-2014.  
This includes team-level total statistics for each game (total field goals attempted, offensive rebounds, etc.)  
The column names should be self-explanatory to basketball fans (as above, **w** or **l** refers to the winning or losing team):  
**wfgm** - field goals made
**wfga** - field goals attempted
**wfgm3** - three pointers made
**wfga3** - three pointers attempted
**wftm** - free throws made
**wfta** - free throws attempted
**wor** - offensive rebounds
**wdr** - defensive rebounds
**wast** - assists
**wto** - time outs
**wstl** - steals
**wblk** - blocks
**wpf** - personal fouls
 
# tourney_compact_results.csv  
This file identifies the game-by-game NCAA tournament results for all seasons of historical data.  
The data is formatted exactly like the **regular_season_compact_results.csv** data.  
Note that these games also include the play-in games (which always occurred on day **134/135**) for those years that had play-in games.  

# tourney_detailed_results.csv  
This file contains the more detailed results for tournament games from 2003 onward.  

# tourney_seeds.csv  
*NB :Assuming it is the official NCAAseed list*  
This file identifies the seeds for all teams in each NCAA tournament, for all seasons of historical data.  
Thus, there are between 64-68 rows for each year, depending on the bracket structure.  
**season** - the year  
**seed** - this is a 3/4-character identifier of the seed, where the first character is either W, X, Y, or Z (identifying the region the team was in) and the next two digits (either 01, 02, ..., 15, or 16) tells you the seed within the region.  
For play-in teams, there is a fourth character (a or b) to further distinguish the seeds, since teams that face each other in the play-in games will have the same first three characters.  
For example, the first record in the file is seed **W01**, which means we are looking at the #1 seed in the W region (which we can see from the **seasons.csv** file was the East region).  
This seed is also referenced in the **tourney_slots.csv** file that tells us which bracket slots face which other bracket slots in which rounds.  
**team** - this identifies the id number of the team, as specified in the **teams.csv** file  

# tourney_slots.csv  
This file identifies the mechanism by which **teams are paired against each other**, depending upon their seeds.  
Because of the existence of play-in games for particular seed numbers, the pairings have small differences from year to year.  

If there were **N** teams in the tournament during a particular year, there were **N-1** teams eliminated (leaving one champion) and therefore **N-1** games played, as well as **N-1** slots in the tournament bracket, and thus there will be **N-1** records in this file for that season.  

**season** - the year  
**slot** - this uniquely identifies one of the tournament games.  

For **play-in games** or **Opening Round game** , it is a three-character string identifying the seed fulfilled by the winning team, such as **W16** or **Z13**.  

On April 22, 2010, the NCAA announced that the tournament would expand to 68 teams, with four **"Play-In Games"** beginning with the 2011 tournament.  
Consequently, the uniqueness of the single, opening round game lasted from **2001–2010**.  
Game played between **two of the lowest-seeded teams** to qualify for an automatic bid to the tournament

For regular tournament games, it is a four-character string, where the first two characters tell you which round the game is (R1, R2, R3, R4, R5, or R6) and the second two characters tell you the expected seed of the favored team.  
Thus the first row is R1W1, identifying the Round 1 game played in the **W** bracket, where the favored team is the 1 seed.  
As a further example, the **R2W1** slot indicates the Round 2 game that would have the 1 seed from the **W** bracket, assuming that all favored teams have won up to that point.  
The slot names are different for the final two rounds, where **R5WX** identifies the national semifinal game between the winners of regions **W** and **X**, and **R5YZ** identifies the national semifinal game between the winners of regions **Y** and **Z**, and **R6CH** identifies the championship game.  
The "slot" value is used in other columns in order to represent the advancement and pairings of winners of previous games.  
**strongseed** - this indicates the expected stronger-seeded team that plays in this game.  
For Round 1 games, a team seed is identified in this column (as listed in the "seed" column in the **tourney_seeds.csv** file), whereas for subsequent games, a slot is identified in this column.  
In the first record of this file (slot **R1W1**), we see that seed **W01** is the "strongseed", which during the 1985 tournament would have been **Georgetown**.  
Whereas for games from Round 2 or later, rather than a team seed, we will see a "slot" referenced in this column.
So in the 33rd record of this file (slot **R2W1**), it tells us that the winners of slots **R1W1** and **R1W8** will face each other in Round 2.  
Of course, in the last few games of the tournament - the national semifinals and finals - it's not really meaningful to talk about a **strong seed** or **weak seed**, but those games are represented in the same format for the sake of uniformity.  
**weakseed** - this indicates the expected weaker-seeded team that plays in this game, assuming all favored teams have won so far.  
For Round 1 games, a team seed is identified in this column (as listed in the "seed" column in the **tourney_seeds.csv** file), whereas for subsequent games, a slot is identified in this column.  
