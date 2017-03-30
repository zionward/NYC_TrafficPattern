# NYC_TrafficPattern
## Initial step
we start with two packages Dinoysus and TDA to do persistent homology....Then we switch to use Igraph library , which can be used to calculate connected component directely
## constructing undirected graph
We then want to detect how link is connected based on error value, according to the merge rule(new component merge to old one), the algorithm works as follows(we focus on error, but can be used for data_trips as well):

    //get qualified link     
    if link_error < critieria:  
        put link in qualified_link_list  
    //Traversal qualifed_link_list
    
    while(qualified_links is not empty)
      
      update qualified_link=traversal_list(fisrt_link,qualified_link)
      new_component=qualified_link(before update)-qualifeid_link(after update)
      add new_component to a list of component //this is the result
        
    func traversal_list(target_link,rest_link):  
        base case1: no link left, return empty  
        base case2: if no anyother links is linked to the target_link, return rest_link  
        remove all links connected with target_link from rest_link  
        recursive case: loop the list containing all links connected with target_link  
                           rest_link=traversal_list(link_in_list,rest_link)
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
            case 1:several links in list of links belong to one component then merge:
                  then prolong the oldest bar of bars which contains several_links
            case 2: single link in list of links belongs to one comonent then update bar
            case 3: components which no link belong to, create new bar for each component
     //result: list of [the largest list of components, start criteria, end criteria]
