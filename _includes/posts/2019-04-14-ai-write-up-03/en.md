> Decision Making

## Control

* Press `1` to switch to the basic motion mode.
* Press `2` to switch to the seeking mode.
* Press `3` to switch to the wandering mode.
* Press `4` to switch to the flocking mode.
* Press `5` to switch to the A star pathfinding mode.
* Press `6` to switch to the decision tree AI mode.
* Press `7` to switch to the behavior tree AI mode. 
* Press `8` to switch to the decision tree learningAI mode.
* Press `J` to use kinematic steering.
* Press `K` to use dynamic steering.  
* Press `D` to open debug rendering (only in astar mode). 

While seeking, press the mouse left button to change the seeking target.

While in pathfinding mode or all decision making modes, click mouse left button to set the aim point. (will fail if click on walls)

## Summary

The assignment is mainly about decision making. The post covers decision trees, behavior trees, and decision tree learning. 

For the decision tree, I implemented both tree class and node class. I used a map to save world states, which can be used as a context between nodes. 

For the behavior tree, I implemented different kinds of nodes, tree, tick, and blackboard. The world state map can also be used in my behavior tree.

I implemented the ID3 algorithm for learning decision trees. The data set for learning is from decision tree instances I made.

In the game, the AI boid will path following to the player or wander around to eat the player. Try to avoid it.

## Details

#### Setup

Before talking about decision trees and behavior trees, there are some setup we need to do. They are world states and behaviors. World states are external inputs of decision making while behaviors are outputs of it.

![](/img/in-post/ai-write-up-03/1.JPG)

***World States***

In this assignment, we regard both character information and world information as external knowledge. And save them in a map. Every time before making a decision, we need to update this map.
 
```c++
typedef map<PropertyType, float> DataMap;
```

The key `PropertyType` is a enum class where some attributes are defined, which can be also used to define the attributes of decision tree nodes and behaviors. Also, we can cast these attributes into intagers for decision tree learning.

***Behaviors***

The output of a decision making instance is behavior. So I defined a behavior base class, and we can define as many derived behavior classes as possible. Each behavior has an only ID. In this way, we can output an id rather than an object. Using Id is also convenient for decision tree learning. We will talk about that later.
```c++
class BehaviorBase
{
public:
	virtual void Action(Boid* i_character, float deltaTime) {};
	uint8_t GetId();

protected:
	uint8_t m_id;
};
```

#### Decision Tree

A Decision tree is a flowchart like tree structure, where each internal node denotes a test on an attribute, each branch represents an outcome of the test, and each leaf node holds an action.

![](/img/in-post/ai-write-up-03/4.jpg)

![](/img/in-post/ai-write-up-03/1.gif)

***Very Big Graph***


#### Behavior Tree

Behaviour Tree is a very popular decision making tool in game development. Campared with decision tree, it's more complex but more reusable and extensible.

![](/img/in-post/ai-write-up-03/3.jpg)

![](/img/in-post/ai-write-up-03/2.gif)


#### Decision Tree Learning

In decision tree learning, we use ID3 (Iterative Dichotomiser 3) algorithm to implemented the learning process. ID3 can generate a decision tree from a dataset.

![](/img/in-post/ai-write-up-03/5.JPG)

![](/img/in-post/ai-write-up-03/3.gif)









## Appendix

[Click to download the Game](/assets/AIAssignment.zip)