#聚类分析 hclust

```
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

# sankeyvalue
```R
library(networkD3)
links<-data.frame(source=c(0,0,1,1,1,2,2,2),target=c(1,2,3,4,5,3,4,5),
                  value=c(23,33,17,3,3,4,13,16))
nodes<-data.frame(name=c('total','0-6分','9-10分','0-6分',
                         '7-8分','9-10分'))
sankeyNetwork(Links = links, Nodes = nodes, Source = "source",
              Target = "target", Value = "value", NodeID = "name",
              units = "TWh", fontSize = 12, nodeWidth = 30)
```

# wordcloud
```R
#1、 首先安装包
#install.packages("wordcloud2")
library(wordcloud2)
# 导入数据
data=data.frame(word=c('上海','江苏','南昌','吉林','太原','北京','哈尔滨'),
                freq=c(9,2,19,14,12,3,3))
miui <- read.csv("miui.csv")
# 为提高速度，可以部分取样
sample_miui=miui[sample(seq(1,length(miui[,1])),100),]
# 支持形状：circle、cardioid\diamond、triangle-forward、triangle、pentagon、star
#wordcloud2(miui, size = 1,shape = 'circle') 
wordcloud2(miui, size = 2, fontFamily = "微软雅黑",
           color = "random-light", backgroundColor = "grey")
# 自定义形状
#wordcloud2(miui, size = 2, fontFamily = "微软雅黑",
#           figPath = 'women.png',
#           color = "random-light", backgroundColor = "grey")

# 文字形式
letterCloud(miui, word = "MIUI", wordSize = 2,color = 'random-dark',backgroundColor = "snow")

```
![wordcloud1](wordcloud_women.png)
