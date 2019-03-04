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

While seeking, press the mouse left button to change the seeking target.

While a star Pathfinding, press mouse left button to set the aim point. (will fail if click on walls)

## Summary

This assignment is mostly about data structure graph and pathfinding algorithms. I implemented the graph class, two pathfinding algorithms, and two heuristics. In this end, I combined pathfinding and steering behavior together to make a scene that supports A* pathfinding.

Compared with the last time, I spent more time on choosing data structure. A good container can save you a lot of running time, and it's easier to finish codes. For example, I used `std::set` to save node records as the open list and the closed list in pathfinding algorithms. It supports quick insertion and erases. Also, it's an ordered container itself. We can easily get the node record that has the lowest cost.

According to profiling, The cost is mainly on render function and `ofColor.Get()` of Openframework. I optimized some classes to get better run-time performance. I believe there is still much stuff I could do to make it faster.

## Details

#### Graph

The data structure used for pathfinding is a weighted singly-connected graph, which only uses positive weights. A graph has some nodes and edges. Here's the structure of them.

```c++
struct Node
{
    uint32_t index;
    ofVec2f position;//to calculate heuristic
    bool isObstacle;
}

class DWEdge
{
    Node* m_sourceNode;
    Node* m_targetNode;
    uint32_t m_cost;
}
```

Consider that we may need more information in a node, I think a struct could be more flexible than only an index. A graph saves a few nodes and edges. We need to find an effective way to save them

To make finding edges more effective, I sorted edges according to source node index first, than according to the target node index. For every node, we'll know all edges source from it once we save the head index in the edge vector and data length, which is an O(1) operation.

```c++
vector<Node> m_nodes;

//edges are ordered while initializing, first sorted by source node, second sorted by target node
vector<DWEdge> m_edges;

//the first is the head index of edges having the same source,
//the second is the number of these edges
typedef pair<uint32_t, uint32_t> EdgeInfo;
//save the number of each source node's outgoing edges and their head index in m_edges; 
vector<EdgeInfo> m_edgeInfo;	
```

Although we need to sort all edges while loading the graph, it only happens once. but we can save lots of time on finding outgoing edges. Or there's a better way, we can sort it while building the graph file, making all data sorted and then we only need to read it in run-time.

***My Graph***

My first graph is a roadmap of a living district. It's somewhere in China. There are 20 nodes in this graph and I assume every neighbor pair of nodes are connected. There are 84 directed edges in total. I evaluated positions of nodes and costs of edges according to their relative positions on the roadmap and input them into a `.txt` file so that my program can read it.

![](/img/in-post/ai-write-up-02/3.jpg)

***Very Big Graph***

I used Python to generate the big graph. it has about 200 nodes and 1000 edges. some nodes are connected while some are not. I just copied and pasted my first graph a few times and make the whole graph connected.

#### Grid

To visualize my pathfinding algorithms, I created another grid class. It can read an image and generate a graph. every node contains some pixels, which can be set by users, and it connected to neighbor nodes. It can detect each pixel's color and determine if it's an obstacle. The grid can draw them in different colors in debug drawing mode so that users can see the "real" graph. 

![](/img/in-post/ai-write-up-02/2.gif)

In this grid, the white part is all path tiles while the pink part is all obstacle tiles. real obstacles are a little wider than the real "walls" to make sure the boid won't get into the wall. the width of a tile is 16 pixels wide. 

By the way, the function `ofColor.Get()` is very expensive because I used it for every pixel of the image. But it seems I don't have other choices.

#### PathFinding

The basic method of pathfinding is to find the next node until you get to the destination. Then how to choose the next node is the key to the algorithm. If you know the cost to every neighbor node, choose the lowest cost way is obviously the best choice. Always choosing the lowest cost we know until the final node is known as the Dijkstra algorithm. Dijkstra algorithm doesn't consider the estimated cost, which makes it cannot find the shortest path for the first time.

If you take estimate cost into account, it will become A* algorithm. Literally, A* algorithm with a good heuristic function can be faster than Dijkstra algorithm because it provides a good prediction to the destination. However, its performance will be worse than Dijkstra if the heuristic function is terrible. We can find that from the running time of two functions. We can also regard the Dijkstra algorithm as a special case of A* algorithm that heuristic is 0.

I use `std::set` as the open list and closed list in both algorithms because it supports quick insert and erases. And you can override compare function so that it can keep all elements sorted. In this way, to find the lowest cost node is also an O(1) operation. Because I also sorted edges in the graph, to get all outgoing edges of a node is also an O(1) operation.

My `NodeRecord` is kind of a linked list so that you can get the path without updating the result vector. But go over the linked list is an O(n) operation, it's still acceptable. I didn't reverse the result but just draw them from the end to the start, which also saves some time.

***Dijkstra***

![](/img/in-post/ai-write-up-02/1.JPG)

***A Star***

![](/img/in-post/ai-write-up-02/2.JPG)
(Manhattan Distance)


#### Heuristic

I used Manhattan distance and Euclidian distance as my heuristics. They are both very good predictions. The difference is that Manhattan distance is based on "blocks" while Euclidian distance is literally linear distance. You can see using Manhattan distance has better performance. I think it is because I set edge cost more in Manhattan way. Anyway, both of them have better performances than the Dijkstra algorithm.

***Manhattan Distance***

**distance = abs(x1 – x2) + abs(y1 – y2)** 
![](/img/in-post/ai-write-up-02/2.JPG)

***Euclidian distance***

**distance = sqrt((x1 * x1 – x2 * x2) + (y1 * y1 – y2 * y2))**
![](/img/in-post/ai-write-up-02/4.JPG)


#### A Star Mode

![](/img/in-post/ai-write-up-02/4.gif)

In the A* pathfinding mode of the program, I used an image as the background and generate a grid object based on it. You can see the tiles it generated above. The grid has about 6000 nodes. If you choose to generate a very long path it's still fast enough even in debug mode. In a grid, every node has the same cost between neighbor nodes so that the only thing you need to check is that if the neighbor node is an obstacle. 

***Path Following Steering***

For dynamic path following steering, I delegated it to dynamic align, dynamic arrive, and dynamic seek.

```c++
	std::vector<Kinematic*>* m_targetsVec;
	float m_targetRadius;
	Kinematic* m_target;
	DynamicSeek* m_dynamicSeek;
	DynamicArrive* m_dynamicArrive;
	DynamicAlign* m_dynamicAlign;
```
At first I thought only dynamic arrive is enough. However, the node is too small so that the boid just move and stop very slowly. I think using dynamic seek for all points other than the destination could be more reasonable. And it did have better performance.

There is a weird bug is that sometimes the boid cannot face to the correct orientation. I checked it and found because it's new orientation is always orthogonal. And the `atan2f()` function I used to calculate orientation sometimes cannot return the correct value. I still need some time to fix it.

## Appendix

[Click to download the Game](/assets/AIAssignment.zip)