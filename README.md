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
 
# let's see if we have communities here using the 
# Grivan-Newman algorithm
# 1st we calculate the edge betweenness, merges, etc...
```
ebc <- edge.betweenness.community(g, directed=F)
memb<- membership(ebc)
```

# Now we have the merges/splits and we need to calculate the modularity
# for each merge for this we'll use a function that for each edge
# removed will create a second graph, check for its membership and use
# that membership to calculate the modularity
```
mods <- sapply(0:ecount(g), function(i){
  g2 <- delete.edges(g, ebc$removed.edges[seq(length=i)])
  cl <- clusters(g2)$membership
  modularity(g,cl)
  
})
```

```
g2 <- delete.edges(G, ebc$removed.edges[1:(which.max(mods)-1)])
memberships$`Edge betweenness` <- clusters(g2)$membership

```
# March 13, 2014 - compute modularity on the original graph g 

# we can now plot all modularities
plot(mods, pch=20)

 
# Now, let's color the nodes according to their membership
g2<-delete.edges(g, ebc$removed.edges[seq(length=which.max(mods)-1)])
V(g)$color=clusters(g2)$membership
 
# Let's choose a layout for the graph
g$layout <- layout.fruchterman.reingold
 
# plot it
plot(g, vertex.label=NA)

```
sizes(ebc)
```
There are 7 communities. this is the size of each one:
![community sizes](https://github.com/yohayn/ex3/blob/master/Images/community_sizes.JPG)
```
modularity(ebc)
```
The modularity value is 0.5774221

