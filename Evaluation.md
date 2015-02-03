#Evaluation  

Submissions are scored on the log loss, also called the predictive binomial deviance:  

**LogLoss=−1n ∑ i=1 n [y i log(y ^  i )+(1−y i )log(1−y ^  i )]**  

where  
n is the **number of games played**  

y ^  i   is the **predicted probability of team 1 beating team 2**  

y i   is **1** if team 1 wins, **0** if team 2 wins  

log()  is the natural (base e) logarithm  
 
A smaller log loss is better.  
Games which are not played are ignored in the scoring.  
**Play-in games are also ignored (only the games among the final 64 teams are scored).**  
The use of the logarithm provides extreme punishments for being both confident and wrong.  
In the worst possible case, a prediction that something is true when it is actually false will add infinite to your error score.  
In order to prevent this, predictions are bounded away from the extremes by a small value.
 
**Submission File**
 
The file you submit will depend on whether the competition is in  
**stage 1** (historical model building) or  
**stage 2** (the 2015 tournament).  

Sample submission files will be provided for both stages.  

The format is a list of every possible matchup between the tournament teams.  

Since team1 vs. team2 is the same as team2 vs. team1, we only include the game pairs where team1 has the lower team id.  
For example, in a tournament of 68 teams (64 + 4 play-in teams), you will predict (68x67)/2  = **2278 matchups**. 
 
Each game has a unique id created by **concatenating** the season in which the game was played, the team1 id, and the team2 id.  
For example, **"2013_1104_1129"** indicates team 1104 played team 1129 in the year 2013.  

You must predict the probability that the team with the lower id beats the team with the higher id.
 
The resulting submission format looks like the following, where "pred" represents the predicted probability that the **first team** will win:  

 id,pred  
 2011_1103_1106,0.5  
 2011_1103_1112,0.5  
 2011_1103_1114,0.5  
 ......  
