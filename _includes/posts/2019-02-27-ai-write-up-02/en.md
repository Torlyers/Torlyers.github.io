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

According to profiling, The cost is mainly on render function and `ofColor.Get()` of openframework. I optimized some classes to get a better run-time performance. I believe there are still many stuff I could do to make it faster.  

## Details

#### Graph

The data structure used for pathfinding is weighted singly-connected graph, which only uses positive weights. A graph has some nodes and edges. Here's the structure of them.
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

Consider that a node could have many functions, I think a struct could be more flexible than only an index. A graph saves a few nodes and edges. To make finding edges more effective, I sorted edges according to source node index first, than according to the target node index. For every node, we'll know all edges source from it once we save the head index in the edge vector and data length.

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

Although we need to sort all edges while loading the graph, it only happen once. but we can save lots of time on finding outgoing edges.

***My Graph***

My first graph is a roadmap of a living district. It's somewhere in China. There are 20 nodes in this graph and I assume every neighbor pair of nodes are connected. There are 84 directed edges in total. I evaluated positions of nodes and costs of edges according their relative positions on the roadmap and input them into a `.txt` file so that my program can read it.

![](/img/in-post/ai-write-up-02/3.jpg)

***Very Big Graph***

I used Python to generate the big graph. it has about 200 nodes and 1000 edges. some nodes are connected while some are not. I just copied and pasted my first graph a few times and make the whole graph connected.

#### Grid

To visualize my pathfinding algorithms, I created another grid class. It can read an image and generate a graph. every node contains some pixels, which can be set by users, and it connected to neighbor nodes. It can detect each pixel's color and determine if it's an obstacle. The grid can draw them in different colors in debug drawing mode so that users can see the "real" graph. 

![](/img/in-post/ai-write-up-02/2.gif)

#### PathFinding


***Dijkstra***

![](/img/in-post/ai-write-up-02/1.JPG)

***A Star***

![](/img/in-post/ai-write-up-02/2.JPG)



#### Heuristic

***Manhattan Distance***



***Euclidian distance***





#### A Star Mode

![](/img/in-post/ai-write-up-02/4.gif)




## Appendix

[Click to download the Game](/assets/AIAssignment.zip)