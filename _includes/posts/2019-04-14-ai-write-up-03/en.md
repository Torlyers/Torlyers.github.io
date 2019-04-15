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

For the decision tree, I implemented both tree class and node class. I used a map to save world states, which can be used as a context between nodes. For the behavior tree, I implemented different kinds of nodes, tree, tick, and blackboard. The world state map can also be used in my behavior tree. I also implemented the ID3 algorithm for learning decision trees. The data set for learning is from decision tree instances I made.

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

A Decision tree is a flowchart-like tree structure, where each internal node denotes a test on an attribute, each branch represents an outcome of the test, and each leaf node holds an action. 

There are two types of nodes in the decision tree. The first one is the decision node, which has an attribute and a test value to make decisions. Decision nodes are not leaf nodes. The second one is action node. All action nodes are leaf nodes and must hold behaviors. I added a special attribute called `Action` in my `PropertyType` enum class, which is enough to distinguish two kinds of nodes. Also, my decision is a binary tree.

```c++
class DTNode
{
public:
    DTNode(PropertyType i_type, DecisionTree* i_tree);
    virtual ~DTNode();

    virtual BehaviorBase* MakeDecision() = 0;
    void CleanUp();

    //setters
    void SetLeft(DTNode* i_node);
    void SetRight(DTNode* i_node);
    void Setbehavior(BehaviorBase* i_behavior);
    void SetPropertyType(PropertyType i_type);

protected:	
    DTNode* m_LeftNode;
    DTNode* m_RightNode;
    DataMap* m_dataMap;//world state
    PropertyType m_propertyType;

    //only for leaf nodes
    BehaviorBase* m_behavior;
};
```

The decision tree holds a root node and recursively call `MakeDecision()` function. I passed a pointer to the global data map so that it can be used to initialize all child nodes.

```c++
class DecisionTree
{
public:
	DecisionTree(DataMap* i_dataMap);

	//setters
	void SetRoot(DTNode* i_node);
	BehaviorBase* MakeDecision();

private:
	DTNode* m_rootNode;
	DataMap* m_dataMap;
};
```

The decision tree instance I use in the game is like this:

![](/img/in-post/ai-write-up-03/4.jpg)

The tree has three decision nodes and three behaviors. First, it will check the distance between AI character and player. If they are far away, the AI character will chase the player. The time mode checks the lasting time of the current state. If the time is too long it will switch to another state. Rotation node checks delta rotation between two characters. While two characters are adjacent to each other, the AI character will start to wander around because I don't want it always path following to the player. And here are the result of my decision tree.

![](/img/in-post/ai-write-up-03/1.gif)

#### Behavior Tree

Behavior Tree is a very popular decision-making tool in game development. Compared with the decision tree, it's more complex but more reusable and extensible. Behavior trees include different kinds of nodes: action node, composite node, and decorator node. And they can have some derived nodes. 

The decision-making process starts from the root node, then executes children nodes until successfully execute an action node. Every node has a state. 

```c++
enum class NodeState
{
	NONE = 0,
	SUCCESS = 1,
	FAILURE = 2,
	RUNNING = 3
};
```

Also, each node has five event functions to do different things. They accept tick object as input. `BTTick` is a class for passing context between neighbor nodes. It has a pointer to the blackboard, which is a block of memory that saves some global data between nodes and trees. It has another pointer to the tree itself so that every node can access to the tree.

```c++
void Enter(BTTick i_tick);
void Open(BTTick i_tick);
NodeState Execute(BTTick i_tick);
void Close(BTTick i_tick);
void Exit(BTTick i_tick);
```

In my behavior tree, I implemented four different nodes: composite node, decorator node, sequence node, and selector node. 

***Sequence Node***
Sequence node is a composite node that can have many children nodes. A composite node's state depends on his children's states. The sequence node will execute all his children in order from left to right. If will return `SUCCESS` only if all his children return `SUCCESS`. It's kind of like `and` operator in programming.

***Selector Node***
Selector node is also a derived class on the composite node. It also executes all his children in order. The difference is that it will stop executing once one child node returns `SUCCESS` and itself will return `SUCCESS` right away. Selector node is like the `or` operator in programming.

***Decorator Node***
Decorator node can only have one child, which is like a condition. It will execute the child if the condition is true. It's kind of a decision tree node so I implemented them in a basic same way. It has an attribute and a test value from the world state.

Here is my behavior tree instance:

![](/img/in-post/ai-write-up-03/3.jpg)

The root node is a selector. The sequencer child, as you can see, doesn't always return `SUCCESS`, so we could regard wandering as the default behavior. When two characters are far from each other, the AI character will go path following. Basically, the behavior is similar to that of the decision tree.

Here is the result of behavior tree.

![](/img/in-post/ai-write-up-03/2.gif)

#### Decision Tree Learning

In decision tree learning, we use ID3 (Iterative Dichotomiser 3) algorithm to implement the learning process. ID3 can generate a decision tree from a dataset.

The basic idea of ID3 is to find out which attribute effects the most in one round, and it should be the attribute in the decision node. Then divide the dataset into several sub-sets according to all values of this attribute. In each round, the best attribute will be regarded. Recursively repeat this process until there are no attributes left. we will get a decision tree.

There are two concepts to evaluate the effect of an attribute. The first one is called entropy. Entropy evaluates the difference in data. If all the data are the same. The entropy will be zero. The second one is called gain. Gain evaluates the entropy in conditions that are values of an attribute. If the entropy declines a lot once we recalculate it in the conditions. We can consider that the attribute effects the most in this round. Here are some codes in the `MakeDecisionTree()` function. It shows the making process

```c++
{
    DTNode* node = new DTNode(PropertyType::Action, i_tree);
    uint8_t ActionId = UINT8_MAX;
    if (CheckAllValuesAreSame(i_table, ActionId))
    {
    	node->SetBehaviorId(ActionId);
    	return node;
    }

    //Find the most effective attribute according to gains
    auto bestAttribute = FindBestAttribute(i_table);
    node->SetPropertyType(bestAttribute);	

    //update tables and attributes
    //...

    //recursively build the tree
    node->SetLeft(MakeDecisionTree(i_tree, leftRemainTable, remainAttributes));
    node->SetRight(MakeDecisionTree(i_tree, rightRemainTable, remainAttributes));

    return node;
}
```

In my implementation, I used 50 data records to train. I didn't save more because my decision executes once per 0.5-second rather than every frame. In the first part of the assignment, I used float values to make decisions like the following left chart. However, The learning only accepts discrete integers as inputs. So I have to transfer float conditions to bool conditions. The last column is the final behavior ID. 

![](/img/in-post/ai-write-up-03/5.JPG)

I did't print the output tree but the behavior of the AI boid is pretty similar to that in the first gif. 

![](/img/in-post/ai-write-up-03/3.gif)