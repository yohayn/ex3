# ex3

# Exercise 3
## Environment setup


```{r}
folder = 'C:/BG/H/Data Scientist/WorkSpace/HW3'
setwd(folder)

```
## Read Data
 loading the graph, using graph.data.frame to convert a dataframe representing an edgelist into an undirected graph.
```{r}
library(igraph)

ga.data <- read.csv('ga_edgelist.csv', header = T)
g <- graph.data.frame(ga.data,directed = F)
```

## 1. a. Computing Centrality

Computing betweenes score for all nodes and printing the actor with the maximum betweeness

```{r}
betweeness <- betweenness(g)
mb <- as.numeric(which(max(betweeness) == betweeness))
V(g)[mb]
```
**Sloan** has the highest betweeness measure 
![sloan](https://github.com/yohayn/ex3/blob/master/Images/sloan.JPG)

Computing closeness score for all nodes and printing the actor with the maximum closeness

```{r}
closeness <- closeness(g)
mc <- as.numeric(which(max(closeness) == closeness))
V(g)[mc]
```

**Torres** has the highest closeness measure 

Computing	Eigencetor score for all nodes and printing the actor with the maximum	Eigencetor

```{r}
evcent <- eigen_centrality(g)
vec <- evcent$vector
me <- as.numeric(which(max(vec) == vec))
V(g)[me]
```

**Karev** has the highest Eigencetor measure 

## 1. b. Community Detection

```{r}
library(igraph)
ga.data <- read.csv('ga_edgelist.csv', header = T)
g <- graph.data.frame(ga.data,directed = F)
```
 
**using Grivan-Newman algorithm**

calculating the edges' betweenness
```{r}
ebc <- edge.betweenness.community(g, directed=F)
memb<- membership(ebc)
```

calculating modularity for each merge by using a function that for each edge
 removed will create a second graph, check for its membership and use
 that membership to calculate the modularity 
```{r}
mods <- sapply(0:ecount(g), function(i){
  g2 <- delete.edges(g, ebc$removed.edges[seq(length=i)])
  cl <- clusters(g2)$membership
  modularity(g,cl)
  
})
```
coloring the nodes

```{r}
g2<-delete.edges(g, ebc$removed.edges[seq(length=which.max(mods)-1)])
V(g)$color=clusters(g2)$membership

```

 
The graph
```{r}
g$layout <- layout.fruchterman.reingold
plot(g)
```
![Grivan-Newman graph](https://github.com/yohayn/ex3/blob/master/Images/Grivan-Newman_algorithm_graph.JPG)

```{r}
sizes(ebc)
```
There are 7 communities. this is the size of each one:

![community sizes](https://github.com/yohayn/ex3/blob/master/Images/community_sizes.JPG)
```{r}
modularity(ebc)
```
The modularity value is 0.5774221


**using Walktrap community finding algorithm**

creating the communities using the algorithm, and returning communities object
```{r}
library(igraph)
ga.data <- read.csv('ga_edgelist.csv', header = T)
g <- graph.data.frame(ga.data,directed = F)
wc <- cluster_walktrap(g)
```
the graph
```{r}
plot(wc, g)
```
![Walktrap graph](https://github.com/yohayn/ex3/blob/master/Images/cluster_walktrap_graph.JPG)

```{r}
sizes(wc)
```
There are 7 communities. this is the size of each one:

![walktrap community sizes](https://github.com/yohayn/ex3/blob/master/Images/walktrap_community_sizes.JPG)
```{r}
modularity(wc)
```
The modularity value is 0.5147059





## 2. Social Network Analysis
Twitter Authentication
```{r}
library(twitteR)
api_key <- "RtgqX9UgBAxe7t2IIM008bCQf"
 
api_secret <- "nHQb98Rqh1bbginMdbUyXQux4BKPekJgLyqXD6Z9cT8Wk2x8WB"
 
access_token <- "811597962870669312-LM03jdYMS6JlJkwrlDFxJsaCPLnTeKE"
 
access_token_secret <- "1uYRzKDIO1F2iK0bDYhGfki8pawHKzGMxQ3SJf8sPJAue"
 
setup_twitter_oauth(api_key,api_secret)
```








a. We chose twitter's API to create a semantic network based on Donald Trump's tweets. we used SocialMediaLab package to collect the tweets. In order to use the API we opened a twitter developer app. 
```{r}
#install.packages('SocialMediaLab')
library(SocialMediaLab)
```
authinticating to twitter API
```{r}
TwitterToken <- Authenticate("twitter", apiKey=api_key, 
                          apiSecret=api_secret,
                          accessToken=access_token, 
                          accessTokenSecret=access_token_secret)
```
Collecting 150 tweets specifying the term "@realDonaldTrump" into a data frame object of class dataSource. 
```{r}
myTwitterData<- Collect(searchTerm="@realDonaldTrump", numTweets=150, 
        writeToFile=FALSE,verbose=TRUE, credential=TwitterToken)
```

converting the tweets' text encodign to utf-8 
```{r}

myTwitterData$text <- iconv(myTwitterData$text, to = 'utf-8')
```
creating a semantic network from the words found in the returned tweets.
```{r}
g_twitter_actor <- Create("Semantic", dataSource=myTwitterData)

gsize(g_twitter_actor)
```

b. there are 78 nodes in the network, each node represents a term, and each edge in the graph represents co-occurence of the terms connected to it in the same tweet.

c.plotting of the graph:
```{r}

library(igraph)
par(mar = rep(2, 4))

plot(g_twitter_actor)
```
![Trump graph](https://github.com/yohayn/ex3/blob/master/Images/Trump_Network.JPG)


## d.Computing Centrality

1.Computing betweenes score for all nodes and printing the words with the maximum betweeness

```{r}
betweeness <- betweenness(g_twitter_actor)
mb <- as.numeric(which(max(betweeness) == betweeness))

V(g_twitter_actor)[mb]
```
**realdonaldtrump** has the highest betweeness measure 

2.Computing closeness score for all nodes and printing the words with the maximum closeness

```{r}
closeness <- closeness(g_twitter_actor)
mc <- as.numeric(which(max(closeness) == closeness))
V(g_twitter_actor)[mc]
```

**realdonaldtrump** has the highest closeness measure 

3.Computing	Eigencetor score for all nodes and printing the words with the maximum	Eigencetor

```{r}
library(igraph)
evcent <- evcent(g_twitter_actor)
vec <- evcent$vector
me <- as.numeric(which(max(vec) == vec))
V(g_twitter_actor)[me]
```

 **trump** has the highest Eigencetor measure 


