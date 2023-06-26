# Plate Appearances and HRs: A Case Study

## Objective

Curiosity killed the cat. 

With this project, I wanted to learn a couple things:

  1. More first-hand experience wrangling, analyzing, and plotting data in the programming language client, R Studio after completing the Google/Coursera Data Analytics Professional Certificate
  2. What makes a hitter an above-average (or below-average) homerun hitter
  3. The difference (if any) in the rate of plate appearances (PA) and the occurance of a homerun (HR)
  4. Eventually, I want to build and evaluate a predictive model to predict how many PAs a player could hit before hitting a homerun. Obviously, the less is better. I feel like this could have practical applications -- for example in betting over/under the amount of homeruns in sports betting.

Eventually, I want to build and evaluate a predictive model to predict how many PAs a player could hit before hitting a homerun. Obviously, the less is better. I feel like this could have practical applications -- for example in betting over/under the amount of homeruns in sports betting.

## Dataset
This project was sourced from 2018-2022 data from Baseball Reference. The dataset included all standard batting stats -- such as HR, games (G), PA, batting average (BA), on-base percentage (OBP), and slugging percentage (SLG) to name a few.

This dataset can be found here for 2018 and can be navigated for the following seasons: https://www.baseball-reference.com/leagues/majors/2018-standard-batting.shtml

## Technologies

The following technologies were used to build this project:
  - Language: R
  - Packages: readr, ggplot

## The data
![alt text](https://github.com/trustinvo/baseball/blob/main/Screenshot%202023-06-26%20at%201.32.19%20PM.png)

This is a sample of what the 2018 table shows. It is nearly identical with 2019-2022, but obviously with different figures.

## The code
````R
library("readr")
batting2018 <- read_csv("2018.csv")
batting2018$PA <- as.numeric(batting2018$PA)
sum_PA18 <- sum(batting2018$PA)
````
This chunk shows the loading of the package that reads .csvs, as well as the import of data. I converted PAs to a numeric and then summed it all up for the league's total. I repeated this step for each season I analyzed.

````R
batting2018$HR <- as.numeric(batting2018$HR)
sum_HR18 <- sum(batting2018$HR)
PA_per_HR_18 <- sum_PA18 / sum_HR18
````
I then summed up the total of HRs and used arithemtic functions to calculate the rate of a PAs per HR. Again, this was repeated for each year. For the sake of reading this blob, I only showed 2018's code chunks.

````R
df <- data.frame(
  Year = c("2018", "2019", "2021", "2020", "2022"),
  PAs_per_HR = c(PA_per_HR_18, PA_per_HR_19,PA_per_HR_20, PA_per_HR_21, PA_per_HR_22)
)
````
The PAs per HR rate was then converted into a data frame for plotting.

## The plot

The numbers were all going to finally mean something to me. In a visually appealing bar graph. We love bar graphs.

I used ggplot2's geom_bar to visually represent my data -- which would answer my questions in one code run.

````R
library("ggplot2")
ggplot(df, aes(x = Year, y = PAs_per_HR)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  labs(x = "Year", y = "PAs / HR") +
  ggtitle("PAs / HR Per Year (League-Average)")
````







