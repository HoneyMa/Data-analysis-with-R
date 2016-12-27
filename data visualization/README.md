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
