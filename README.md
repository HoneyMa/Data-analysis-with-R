# 第二步：数据分析
## 2.Descriptive statistics
### 多组散点图
```R
f <- factor(iris$Speices)
with(iris, plot(Petal.Length, Petal.Width, pch = as.interger(f)))
legend(1.5, 2.4, as.character(levels(f)), pch=1:length(levels(f)))
```
![](irisRplot.png)

## 3.Inferencial statistics
### 3.1 t test
### 3.2 The Analysis of Variance
One-way ANOVA
```R
attach(iris)
names(iris)
aggregate(Sepal.Length, by=list(Species), FUN=mean)
aggregate(Sepal.Length, by=list(Species), FUN=sd)
fit <- aov(Sepal.Length ~ Species)
summary(fit)
detach(iris)
```
### 3.3 Cluster analysis
