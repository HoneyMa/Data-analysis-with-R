# 第二步：数据分析
## 1.Descriptive statistics
### 1.1 多组散点图
```R
f <- factor(iris$Speices)
with(iris, plot(Petal.Length, Petal.Width, pch = as.interger(f)))
legend(1.5, 2.4, as.character(levels(f)), pch=1:length(levels(f)))
gird()#绘制网格
```
![](irisRplot.png)
### 每个因子水平下一副散点图
```R
data(Cars93, package="MASS)
coplot(Horsepower ~ MPG.city | Origin, data=Cars93)
```
### 改变颜色
图像上方与下方分别用不同的颜色显示
``` R
> x = runif(20, min=-3, max = 3)
> plot(x, type = "h", lwd=3)
> colors <- ifelse(x >=0, "black", "gray")
> plot(x, type = "h", lwd=3, col=colors)
```

### 2.1 条形图
在条形图上面添加置信区间，使用gplots包中的barplot2。
```R
library(gplots)
attach(airquality) # 这是一个关于空气质量的数据包
```
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6
```R
heights <- tapply(Temp, Month, mean)
lower <- tapply(Temp, Month, function(v) t.test(v)$conf.int[1])
upper <- tapply(Temp, Month, function(v) t.test(v)$conf.int[2])
barplot2(heights, plot.ci = T, ci.l = lower, ci.u = upper)
barplot2(heights, plot.ci = T, ci.l = lower, ci.u = upper,
          ylim=c(50,90), xpd=FALSE,
          main = "Mean Temp. By Month",
          names.arg = c("May","Jun", "Jul", "Aug", "Sep"), #改变坐标轴的文字
          ylab = "Temp(deg.F)")
```

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

```R
#导入数据，并将数据中转换为矩阵
data=iris[,-5]
dist.e=dist(data,method='euclidean')
# 热力图
heatmap(as.matrix(dist.e),labRow = F, labCol = F)
#聚类 并将结果限制为3类
model1=hclust(dist.e,method='ward')
result=cutree(model1,k=3)
#使用MDS降维
mds=cmdscale(dist.e,k=2,eig=T)
x = mds$points[,1]
y = mds$points[,2]
#通过ggplot2可视化的表现出来
library(ggplot2)
p=ggplot(data.frame(x,y),aes(x,y))
p+geom_point(size=3,alpha=0.8,
             aes(colour=factor(result),
               shape=iris$Species))

```
另外一个例子
```R
#案例:对multcomp包中的怀孕小鼠数据做聚类

library(multcomp)
#先通过图表了解数据集的分布
library(ggplot2)
data(litter, package="multcomp" )
#不同剂量组别的小鼠，产出的幼崽体重分布
ggplot(data = litter, package="multcomp",
       aes(x=weight, fill=dose)) +
  geom_density(alpha=.3) 
#不同剂量组别的小鼠，产出的幼崽个数分布
ggplot(data = litter, package="multcomp",
       aes(x=number, fill=dose)) +
  geom_density(alpha=.5) 
#表示协变量
ggplot(data = litter, 
       aes(x=number, fill=dose)) +
  geom_density(alpha=.5) +
  facet_grid(.~gesttime)
#散点图
ggplot(data = litter, package = "multcomp", 
       aes(x=number, y = weight, 
           colour = factor(dose))) +
  geom_point() 

### 对小鼠进行聚类
# step1: 选择合适的变量
mouse <- litter[,-1]
# step2: 对数据进行标准化/转化
z <- scale(mouse)
# step3:寻找异常点

#step4: 计算距离
d <- dist(z)

#step5:选择聚类方法
fit.average <- hclust(d, method = "average")
plot(fit.average, hang =-1, cex=.8, main = "AVERAGE LINKAGE CLUSTERING")
rect.hclust(fit.average, k=4) # 在图上将聚类结果框起来

#step6:确定类的数目
clusters <- cutree(fit.average, k=4)
table(clusters) # 查看每类里面有多少观测值
aggregate(mouse, by=list(cluster=clusters), median) #描述聚类
aggregate(as.data.frame(z), by=list(cluster=clusters), median) #描述聚类

#step7:尝试多种聚类方法,并确定最终的聚类方法
fit.single <- hclust(d, method = "single")
plot(fit.single, hang =-1, cex=.8, main = "Single LINKAGE CLUSTERING")

fit.complete <- hclust(d, method = "complete")
plot(fit.complete, hang =-1, cex=.8, main = "complete LINKAGE CLUSTERING")

fit.ward <- hclust(d, method = "ward")
plot(fit.ward, hang =-1, cex=.8, main = "ward LINKAGE CLUSTERING")
clusters.ward <- cutree(fit.ward, k=3)

#step8:结果的可视化
#使用MDS降维
mds=cmdscale(d,k=2,eig=T)
x = mds$points[,1]
y = mds$points[,2]
#通过ggplot2可视化的表现出来
library(ggplot2)
p=ggplot(data.frame(x,y),aes(x,y))
p+geom_point(size=3,alpha=0.8,
             aes(colour=factor(clusters),
                 shape=litter$dose))
#ward法
p=ggplot(data.frame(x,y),aes(x,y))
p+geom_point(size=3,alpha=0.8,
             aes(colour=factor(clusters.ward),
                 shape=factor(litter$gesttime))
             )
```
###因素分析
```R
rawdata <- read.table("data.csv", header = T, sep = ",")
# 数据清理 先了解数据的整体情况，缺失值，极端值
# 剔除答题时间特别长的或者特别短的问卷
out <- boxplot(rawdata$所用时间.秒.) # $out列出了极端值
# 观察极端值数据是否异常
# 把极端值提出观测
newdata <- rawdata[rawdata$所用时间.秒. >50 & rawdata$所用时间.秒. < 1833,]
plot(density(newdata$所用时间.秒.))

# 通过交叉表了解数据分布情况
table(rawdata$Q35, rawdata$Q36)

####对12题做因素分析，希望能够决定购买的意思做降维 
dtq12 <- rawdata[c("Q12_A1", "Q12_A2","Q12_A3","Q12_A4","Q12_A5",
                   "Q12_A6", "Q12_A7","Q12_A8","Q12_A9","Q12_A10",
                   "Q12_A11", "Q12_A12","Q12_A13","Q12_A14")]
#利用JSON串替换列名
library('rjson')
code <- fromJSON(file="code.json")
items <- c(code$Q12$code$"Q12_A1", code$Q12$code$"Q12_A2",code$Q12$code$"Q12_A3", code$Q12$code$"Q12_A4",
           code$Q12$code$"Q12_A5", code$Q12$code$"Q12_A6",code$Q12$code$"Q12_A7", code$Q12$code$"Q12_A8",
           code$Q12$code$"Q12_A9", code$Q12$code$"Q12_A10",code$Q12$code$"Q12_A11", code$Q12$code$"Q12_A12",
           code$Q12$code$"Q12_A13", code$Q12$code$"Q12_A14")
names(dtq12)[1:14] <- items 
#精简代码 我不想手动的敲这么多代码，可以直接索引需要的值
w <- code$Q12$code   # 可以获得所有选项对应的内容，但是顺序是随机的，没有按照序号顺序
qlist <- code$Q12$qlist #qlist 可以获得正确的选项序号，但是没有选项的内容
# 这里我们需要将qlist的正确顺序与w的内容对应起来
# w[qlist[1]] #获得qlist里中第一个元素的列名，如Q1_A1，然后在w里面找q1_a1的对应内容
names(dtq12)[1:14]<-w[qlist] # 这里我们按顺序索引了qlist里面所有的列名，然后再一一对应其在w里面相应的内容

#将缺失值替换为5
dtq12[is.na(dtq12)] <- 5
#获得相关矩阵
core <- round(cor(dtq12),2)
#将相关矩阵保存在excel中
write.csv(core, "core.csv")

#因子分析
library(psych)
principal(core)
fa.parallel(core, fa="both", n.iter = 357, show.legend = T)
principal(core,nfactors = 6)


mydata <- read.table("dd.csv", header = T, sep = ",")
mydata <- na.omit(mydata)
chisq.test(mydata)
fisher.test(mydata)
t.test(mydata)
aov(mydata)

```
## 对应分析
发现一篇文章介绍的比较详细：http://blog.sina.com.cn/s/blog_5a8239800100uq7t.html
