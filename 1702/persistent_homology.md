# NYC_TrafficPattern
## Initial step
we start with two packages Dinoysus and TDA to do persistent homology but they required higher dimentional data sets. Then we switch to use Igraph library, which can be used to calculate components. We generated bar histogram with the help of Igraph.

## constructing undirected graph
We then want to detect how link is connected based on error value, according to the merge rule(new component merge to old one), the algorithm works as follows:

    //get qualified link     
    if link_error < critieria:  
        put link in qualified_link_list  
    //Traversal qualifed_link_list
    
    while(qualified_link_list is not empty)
      
      update qualified_link_list=traversal_list(first_link in qualified_link_list,qualified_link)
      new_component=qualified_link_list(before update)-qualified_link_list(after update)
      add new_component to a list of component //this is the result
        
    func traversal_list(target_link,rest_link):  
        base case1: no link left, return empty  
        base case2: if no links are linked to the target_link, return rest_link  
        remove all links connected with target_link from rest_link  
        recursive case: rest_link=traversal_list(link_in_list,rest_link)
        return rest_link
        
    //plot bar code
    count_result=list of list of components under each critiera
    loop criteriera:
    check if component existis 
    if the first component appear
        create bar for each component under curr_critieria
    else 
        list of links=get a random link from each component under curr_critieria
        find which component of next_critieria the link belongs to
            case 1:if several links in list of links belong to one component then merge:
                  then prolong the oldest bar of bars which contains several_links
            case 2: single link in list of links belongs to one comonent then update bar
            case 3: components which no link belong to, create new bar for each component
     //result: list of [the largest list of components, start criteria, end criteria]

## Constructing Directed Graph
Since undirected graph ignores directions, when two links have the same begin node but different end node, it was counted as a single component. We begin to count components under directed graph.

![Picture](https://github.com/zionward/NYC_TrafficPattern/blob/master/1702/diagram/Untitled%20Diagram.png?raw=true)

## Preliminary
A directed graph is strongly connected if there is a directed path from any vertex to any other. A directed graph is acyclic if it does not contain a directed cycle; directed acyclic graphs are often called dags.
Any vertex in a dag that has no incoming vertices is called a source; any vertex with no outgoing edges is called a sink. 
Let C be any strong component of G that is a sink in scc(G)->C is a sink component.

## Counting Strongly Connected Components
We use the package tarjan to get scc then use DFS algorithms to visit each node.
```
    scc(v):
        Find qualified links
        Build graph based on vertice and links D
        scc <- tarjan(D)
```

## Construcing Meta Graph
The scc considered the case when node can go back to iteself as a component. However, New York City has many one way roads, which are all ignored as components if we use scc as base. Therefore, the component criteria is that either it is a directed cyclic graph where source node can go back to itself, or it can go from source node to sink node. 
 
1. Find all start links.
    Start Links: A link whose start node is not end node of any other links.
2. Traverse the graph starting from start links by DFS (Depth-First-Search) Algorithm, at the same time using counting path method below to count path.
    https://en.wikipedia.org/wiki/Depth-first_search
3. From the previous SCC step, find all SCC in the graph. Shrink every SCC into a single node.
4. Traverse the Graph starting from each SCC components, which have been shrinked into nodes, at the same time using counting path method below to count path.

### Counting path method:
Using Pascal's triangle  to count path. Increase the counting everytime when more than one links meets together. Adds the counting of those links together.

Example:
    1->->
        2->-> 
    1->->
    Here we have 2 path at the end.

    1->->
        2->->
    1->->   4->->
        2->->
    1->->
    Here we have 4 path at the end.
