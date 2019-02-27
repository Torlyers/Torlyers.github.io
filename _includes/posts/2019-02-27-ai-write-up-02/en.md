> Graph & Pathfinding

## Control

* Press `1` to switch to the basic motion mode.
* Press `2` to switch to the seeking mode.
* Press `3` to switch to the wandering mode.
* Press `4` to switch to the flocking mode.
* Press `5` to switch to the A star pathfinding mode.
* Press `6` to use kinematic steering.
* Press `7` to use dynamic steering. 
* Press `8` to use switch leader. 
* Press `D` to open debug rendering (only in astar mode). 

While seeking, press mouse left button to change the seeking target.
While a star Pathfinding, press mouse left button to set the aim point. (will fail if click on walls)

## Summary

This assignment is mostly about data structure graph and pathfinding algorithms. I implemented graph class, two path finding algorithms, and two heuristics. In this end, I combined path finding and steering behavior together to make a scene that support a star path finding.

Compared with the last time, I spent more time on choosing data structure. A good container can save you a lot of running time, and it's easier to finish codes. For example, I used `std::set` to save node records as open list and close list in pathfinding algorithms. It supports quick insertion and erase. Also, it's an ordered container itself. We can easily get the node record that has the lowest cost.

Optimization is very important. My A Star mode is kind of slow in debug mode. It worked well in release mode. I tried some methods to save the cost.

## Details

#### Graph

***My Graph***


***Very Big Graph***


#### Grid




#### PathFinding


***Dijkstra***

![](/img/in-post/ai-write-up-02/1.JPG)

***A Star***

![](/img/in-post/ai-write-up-02/2.JPG)



#### Heuristic

***Manhattan Distance***

![](/img/in-post/ai-write-up-01/5.gif)

***Euclidian distance***

![](/img/in-post/ai-write-up-01/2.gif)



#### A Star Mode




## Appendix

[Click to download the Game](/assets/AIAssignment.zip)