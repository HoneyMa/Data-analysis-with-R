# 第三步：数据可视化

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

#![wordcloud1](wordcloud_women.png)
```
 <img src="https://github.com/HoneyMa/Data-analysis-with-R/blob/master/data%20visualization/wordcloud_women.png" width = "500" height = "500" alt="地图数据" align=center />

#地图数据可视化
参考文章：http://cos.name/2009/07/drawing-china-map-using-r/
百度的在线编辑器： http://echarts.baidu.com/gallery/editor.html?c=map-china-dataRange
```R
# map
library(maps)
library(ggplot2)
library(mapdata)
map("china")  # 画出来了中国地图
library(maptools)
x <- readShapePoly("bou2_4p.shp")
plot(x) # 画出了有重庆的中国地图

mydata <- read.csv("地图数据.csv", header = TRUE)
provname <- as.character(mydata$省份) #省份的名称需要时全称
pop <- mydata$LOG个数       #省份对应的人数

getColor=function(mapdata,provname,provcol,othercol)
  {
  f=function(x,y) ifelse(x %in% y,which(y==x),0);
  colIndex=sapply(mapdata@data$NAME,f,provname);
  col=c(othercol,provcol)[colIndex+1];
  return(col);
  } #颜色的函数 

provcol=rgb(red=0,green=1-pop/max(pop)/1.5,blue=1-pop/max(pop)/1.5)
plot(x,col=getColor(x,provname,provcol,"white"),xlab="",ylab="")


#provname=c("北京市","天津市","上海市","重庆市")
#provcol=c("red","green","yellow","purple")
#plot(x,col=getColor(x,provname,provcol,"white"))

#provname=c("北京市","天津市","河北省","山西省","内蒙古自治区",
#           "辽宁省","吉林省","黑龙江省","上海市","江苏省",
#           "浙江省","安徽省","福建省","江西省","山东省",
#           "河南省","湖北省","湖南省","广东省",
#           "广西壮族自治区","海南省","重庆市","四川省","贵州省",
#           "云南省","西藏自治区","陕西省","甘肃省","青海省",
#           "宁夏回族自治区","新疆维吾尔自治区","台湾省",
#           "香港特别行政区")
#pop=c(1633,1115,6943,3393,2405,4298,2730,3824,1858,7625,
#      5060,6118,3581,4368,9367,9360,5699,6355,9449,
#      4768,845,2816,8127,3762,4514,284,3748,2617,
#      552,610,2095,2296,693)
```
 <img src="https://github.com/HoneyMa/Data-analysis-with-R/blob/master/data%20visualization/%E5%9C%B0%E5%9B%BE%E6%95%B0%E6%8D%AE.png" width = "500" height = "500" alt="地图数据" align=center />
 
 用百度的编辑器 把原始数据取对数
  <img src=" https://github.com/HoneyMa/Data-analysis-with-R/blob/master/data%20visualization/pic/%E8%B0%83%E7%A0%94%E6%A0%B7%E6%9C%AC%E5%88%86%E5%B8%83%E6%83%85%E5%86%B5.png" width = "500" alt="地图数据" align=center />
