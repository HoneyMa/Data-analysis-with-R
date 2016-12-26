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
