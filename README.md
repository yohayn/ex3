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

## 1. b. Community Detection

```library(igraph)
ga.data <- read.csv('ga_edgelist.csv', header = T)
g <- graph.data.frame(ga.data,directed = F)
```
 
**using Grivan-Newman algorithm**
calculating the edges' betweenness
```
ebc <- edge.betweenness.community(g, directed=F)
memb<- membership(ebc)
```

calculating modularity for each merge by using a function that for each edge
 removed will create a second graph, check for its membership and use
 that membership to calculate the modularity 
```
mods <- sapply(0:ecount(g), function(i){
  g2 <- delete.edges(g, ebc$removed.edges[seq(length=i)])
  cl <- clusters(g2)$membership
  modularity(g,cl)
  
})
```
coloring the nodes

```
g2<-delete.edges(g, ebc$removed.edges[seq(length=which.max(mods)-1)])
V(g)$color=clusters(g2)$membership

```

 

 
 
The graph
```
g$layout <- layout.fruchterman.reingold
plot(g, vertex.label=NA)
```
![Grivan-Newman graph](https://github.com/yohayn/ex3/blob/master/Images/Grivan-Newman_algorithm_graph.JPG)

```
sizes(ebc)
```
There are 7 communities. this is the size of each one:
![community sizes](https://github.com/yohayn/ex3/blob/master/Images/community_sizes.JPG)
```
modularity(ebc)
```
The modularity value is 0.5774221

**using Walktrap community finding algorithm**
creating the communities using the algorithm, and returning communities object
```{r}
wc <- cluster_walktrap(g)
```
the graph
```
plot(wc, g)
```
![Walktrap graph](https://github.com/yohayn/ex3/blob/master/Images/cluster_walktrap_graph.JPG)

```
sizes(wc)
```
cluster_walktrap_graph.JPG
There are 7 communities. this is the size of each one:
![community sizes](https://github.com/yohayn/ex3/blob/master/Images/walktrap_community_sizes.JPG)
```
modularity(wc)
```
The modularity value is 0.5774221

