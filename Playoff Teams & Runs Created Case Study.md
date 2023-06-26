# Playoff Teams & Runs Created: A Love Story .. I mean Case Study

## Objective

It is no surprise the name of the game of baseball is to outscore the opponent. When you outscore the opponent for a whole game, it counts as a win. So to win, you must create more runs than the other team. Runs Created (RC) was a sabermetric statistic created by the late-great Bill James. According to MLB.com, it estimates a player's offensive contribution in terms of total runs. It combines a player's ability to get on base with his ability to hit for extra bases and divides those two by the player's total opportunities. The formula for Runs Created is: TB [total bases] x (H [hits] + BB [walks]) / (AB [at-bats] + BB [walks]). 

This fascinated me because we know a good team outscores their opponent. In other words, if you create more runs than the other team, you win. I wanted to see the relationship between wins and RC.

## Technologies

Languages: R, SQL
Packages: readr, ggplot2

## Dataset
I turned to Baseball Reference again for the sabermetric statistics from 2018. I collected 2018-2021 for this case study as well, but have not done the analysis for 2019-2022 quite yet. I will soon. I'm busy writing this blob. The 2018 sabermetric statistics table shows these fun stats for all 30 teams:

![alt text](https://github.com/trustinvo/baseball/blob/main/Screenshot%202023-06-26%20at%202.38.55%20PM.png)

There was one problem -- wins are no where to be found in this dataset. I turned to Baseball Reference's standings statistics from that year:

![alt text](https://github.com/trustinvo/baseball/blob/main/Screenshot%202023-06-26%20at%202.41.49%20PM.png)

Now, another problem. I could manually enter or copy/paste in wins to the matching team. Or I could utilize what I've learned in SQL all this time. Don't threaten me with a good time -- I'm turning to SQL.

## Data wrangling
I made the sabermetric table and the wins/losses tables into .csv files and named them accordingly, SABR and WL. 
````SQL
select  WL.W, SABR.Tm, SABR.RC, SABR.lgOBP, SABR.lgSLG
from `2018_2022mlbdata.SABR_2018` SABR
left join `2018_2022mlbdata.WL_2018` WL on SABR.Tm = WL.Tm
where SABR.Tm is not null AND WL.Tm Not Like '%league average%'
````
There were some league average totals in there that I didn't want so I filtered them out. I joined the sabermetric table and the wins/losses table to attach the team name, wins, and runs created into one table. I also added lgOBP and lgSLG statistics because in the future I may use them to build a predictive model. I exported this SQL table into a .csv and then added a "playoffs" columbn because the goal of this study was to find out how do RC and playoff qualification correlate. After all, to make the playoffs, you need to win.

This is how the table looks: 
![alt text](https://github.com/trustinvo/baseball/blob/main/Screenshot%202023-06-26%20at%202.51.37%20PM.png)

I made a table for each season I wanted to analyze (2018-2022), then imported into R Studio.

## R Studio
````R
library("readr")
MLB_2018 <- read.csv("MLB_2018.csv")
MLB_2019 <- read.csv("MLB_2019.csv")
MLB_2020 <- read.csv("MLB_2020.csv")
MLB_2021 <- read.csv("MLB_2021.csv")
MLB_2022 <- read.csv("MLB_2022.csv")

library("ggplot2")

ggplot(data = MLB_2018, aes(x = RC, y = W)) +
  geom_point(aes(color = ifelse(playoffs == "yes", "Playoffs", "Did not qualify")), size = 3) +
  xlab("Runs Created") +
  ylab("Wins") +
  ggtitle("MLB 2018: Runs Created vs Wins") +
  scale_color_manual(values = c("Playoffs" = "red", "Did not qualify" = "blue"), labels = c("No", "Yes")) +
  labs(color = "Playoffs")
````

This code spat out a beautiful visualization that highlights playoff teams and runs created and how they compares to non-playoff teams.

![alt text](https://github.com/trustinvo/baseball/blob/main/Image%206-23-23%20at%204.26%20PM.jpg)

## Conclusion

I was able to conclude that playoff teams are in the upper echelon of runs created. While it would be useful to look at runs differential, we know that from this graph, offense is very important. The teams that qualified for the playoffs led the league in runs created. This has implications that I can predict runs created by building and evaluating a predictive model and how it can be predict a possible playoff berth.

## Limitations

Some limitations we know of is that runs created isn't the only factor in deciding factor in whether or not a team is playoff bound. Runs differential (aka factoring in pitching/defense) can also paint a picture. Divisional/wild card standings are also important to note, because a team can be top in runs created but still be behind the top 2 better teams in their division, missing the playoffs. Nonetheless, this study painted a picture that I thought was interesting -- make no mistake: playoff teams can cross the hell out of home plate and do so by creating opportunities. Sort of a life lesson we can all apply to life.



