# Data-analysis-with-R
> This is my study notes of data analysis with R

## 1.Basic data management

### 1.1 Data Input
There're 3 ways to get your data ready in R.
First, you may call a edit window, like :
```R
roster <- data.frame(Student=character(0), Math=numeric(0), Science=numeric(0), English=numeric(0))
roster <- edit(roster)
```
Second, you may create seperate vectors and then integrate them into a dataframe or something. like:
```R
manager <- c(1,2,3,4,5)
date <- c("10/24/08", "10/28/08", "10/1/08", "10/12/08", "5/1/09")
country <- c("US", "US", "UK", "UK", "UK")
gender <- c("M", "F", "F", "M", "F")
age <- c(32, 45, 25, 39, 99)
q1 <- c(5, 3, 3, 3, 2)
q2 <- c(4, 5, 5, 3, 2)
q3 <- c(5, 2, 5, 4, 1)
q4 <- c(5, 5, 5, NA, 2)
q5 <- c(5, 5, 2, NA, 1)
leadership <- data.frame(manager, date, country, gender, age,
                         q1, q2, q3, q4, q5, stringsAsFactors = FALSE)
```
Last, you could import files like csv, spss, excel etc. in R.

### 1.2 Data Recode
Sometimes you need recode the numeric data to category data for analyze. Like:
```R
leadership$age[leadership$age == 99] <- NA
leadership$agecat [leadership$age > 75] <- "Elder"
leadership$agecat [leadership$age >=55 & leadership$age <= 75] <- "Middle Aged"
leadership$agecat [leadership$age < 55] <- "Young"
```
