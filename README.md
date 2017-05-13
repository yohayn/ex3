# Exercise 3
## Environment setup
```{r}
folder = 'C:/BG/H/Data Scientist/WorkSpace/HW3'
setwd(folder)

```
## Read Data
 loading the graph, using graph.data.frame to convert a dataframe representing an edgelist into an undirected graph.
```
library(igraph)

ga.data <- read.csv('ga_edgelist.csv', header = T)
g <- graph.data.frame(ga.data,directed = F)
```

## Setting a layout
Choosing a layout scheme and plotting the networks.

```
g$layout <- layout.fruchterman.reingold(g)
plot(g)
```

## Setting a layout
Computing betweenes score for all nodes
```
g$layout <- layout.fruchterman.reingold(g)
plot(g)
```

![str result Image](https://github.com/yohayn/ex3/blob/master/images/sloan.JPG)
