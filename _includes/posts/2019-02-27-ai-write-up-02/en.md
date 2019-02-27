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




## Data Structure

#### Graph


#### Grid


#### PathFinding




## Algorithms

#### Create Graph


#### PathFinding

***A Star***

![](/img/in-post/ai-write-up-01/4.gif)

***Dijkstra***

![](/img/in-post/ai-write-up-01/1.gif)



#### Heuristic

***Manhattan Distance***

![](/img/in-post/ai-write-up-01/5.gif)

***Euclidian distance***

![](/img/in-post/ai-write-up-01/2.gif)



#### A Star Mode




## Appendix

[Click to download the Game](/assets/AIAssignment.zip)