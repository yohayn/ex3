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

## 1. a. Computing Centrality

Computing betweenes score for all nodes and printing the character with the maximum betweeness

```
betweeness <- betweenness(g)
mb <- as.numeric(which(max(betweeness) == betweeness))
V(g)[mb]
```
**Sloan** has the highest betweeness measure 
![sloan](https://github.com/yohayn/ex3/blob/master/Images/sloan.JPG)

Computing closeness score for all nodes and printing the character with the maximum closeness

```
closeness <- closeness(g)
mc <- as.numeric(which(max(closeness) == closeness))
V(g)[mc]
```

**Torres** has the highest closeness measure 

Computing	Eigencetor score for all nodes and printing the character with the maximum	Eigencetor

```
evcent <- evcent(g)
vec <- evcent$vector
me <- as.numeric(which(max(vec) == vec))
V(g)[me]
```

**Karev** has the highest Eigencetor measure 
