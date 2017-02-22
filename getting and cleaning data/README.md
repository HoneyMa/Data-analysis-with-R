http://www.r-tutor.com/
http://statmethods.net/input/dates.html

# 第一步：数据准备
## 1.Basic data management
### 1.1 Data Input 数据导入
There're 3 ways to get your data ready in R.
First, you may call a edit window, like :
```R
roster <- data.frame(Student=character(0), Math=numeric(0), Science=numeric(0), English=numeric(0))
roster <- edit(roster)
# tips: make sure to save your dataset(here, which is roster) to local
save(roster, file = "x.RData")
# from csv 
data=read.csv('filename.csv')
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

### 1.2 Data Recode 数据重新编码
Sometimes you need recode the numeric data to category data for analyze. Like:
```R
leadership$age[leadership$age == 99] <- NA
leadership$agecat [leadership$age > 75] <- "Elder"
leadership$agecat [leadership$age >=55 & leadership$age <= 75] <- "Middle Aged"
leadership$agecat [leadership$age < 55] <- "Young"
```
### 1.3 Rename the variable 变量重命名
```R
names(leadership)[1] <- "ManagerID"
names(leadership)[2] <- "testDate"
names(leadership)[6:10] <- c("item1", "item2", "item3", "item4", "item5")
```
### 1.4 Select cases from the dataset 从数据集中选择个案/索引
You may select some cases or obversations from the whole dataset for further analyze, use this:
```R
newdata <- leadership[leadership$gender == "M" & leadership$age >30]
```
### 1.5 Case study: Comparision of students' grades in different subjects
You need determine the overall performance of several students by their grades in English, Math, and Science, which were scored with different standards.
```R
# data input
roster <- data.frame(Student=character(0), Math=numeric(0), Science=numeric(0), English=numeric(0))
roster <- edit(roster)

# data standorization for grades
z <- scale(roster[,2:4])

# compute the mean of each row/student. and add this new variable to roster 
score <- apply(z, 1, mean)
roster <- cbind(roster, score)

# get the threshold for different levels, so that you can make your decision 
y <- quantile (roster$score, c(.8,.6,.4,.2))
roster$grade[score >= y[1]] <- "A"
roster$grade[score < y[1] & score >= y[2]] <- "B"
roster$grade[score < y[2] & score >= y[3]] <- "C"
roster$grade[score < y[3] & score >= y[4]] <- "D"
roster$grade[score < y[4]] <- "F"

#split the name as first name and last name(which only works when the variable is a character)
roster$Student <- as.character(roster$Student)
name <- strsplit((roster$Student), " ")

# Create new variables of Lastname and firstname, and add them to roster meanwhile delete Student
Lastname <- sapply(name, "[", 2)
Firstname <- sapply(name, "[", 1)
roster <- cbind(Lastname, Firstname, roster[,-1])

# rank by lastname and firstname
roster <- roster[order(Lastname, Firstname), ]
```
### 1.6 数据转换
目的|数据类型|函数
----|----|-------
向量分组||split(), unstack()|
将函数应用于每个列表元素|list|lapply(lst,fun)返回列表，sapply()返回向量或矩阵|
将函数应用于矩阵每行|matrix|results <- apply(matrix, 1, fun) |
将函数应用于矩阵每列|matrix|results <- apply(matrix, 2, fun) |
将函数应用于组数据|向量，分组因子|tapply(x,f,fun)
将函数应用于行组|dataframe|by(dfrm, fact, fun) fact是分组因子
将函数应用于平行向量或列表||mapply



##2 数据重构/数据透视/reshape2
  分析多选题时（每个选项0/1编码）遇到一个问题，在进行统计分析之前，需要获得分类变量的交叉频数表。
  本来，在excel中可通过数据透视表获得交叉频数表，但这table不知如何导入R。
  如果是单选题，可以在R中使用table函数获得频次表，但是多选题就无从下手了。在JSONG的提示下，试着在R中实现数据透视表的操作。
  原来竟是用reshape2。
  ```R
  library(reshape2)
  redata <- read.csv("reshape.csv", header = TRUE, sep = ",")
  mdata <- melt(redata, id.vars = c("ID", "性别年龄"), variable.name = "factors")
  mdata <- mdata[order(mdata$性别年龄),]
  cdata <- dcast(mdata, 性别年龄~ factors, sum) # 获得性别年龄×购买影响因素的交叉频次表 重要！！这里变量名上不需要加引号
  ```
  
