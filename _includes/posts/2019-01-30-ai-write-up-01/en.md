> Steering Algorithms


## Control

* Press `1` to switch to the basic motion mode.
* Press `2` to switch to the seeking mode.
* Press `3` to switch to the wandering mode.
* Press `4` to switch to the flocking mode.
* Press `5` to use kinematic steering.
* Press `6` to use dynamic steering. 

While seeking, press mouse left button to change the seeking target.


## Summary

This assignment includes two parts. The first part is to develop a game framework. I implemented gameobjects, kinematics, and steering components. They are well encaspulated and have user-friendly interfaces. The second part is steering algorithms. I implemented more than 10 steering algorithms based on physics, and combined them into four motion modes.  

In the process, I thought a lot about algorithms and the framework. Some of the algorithms can have something wrong or can be improved. While blending steeing outputs, I spent much time on how to set parameters. Another thing is to unify interfaces for all steering objects. I used an intermediate class `SteeringComponent` to manage all steering classes. In short, it's hard to get a perfect code structure and outcome. I did my best and I think it works well.

## Framework

#### Game

The game is based on `Openframeworks`, which has a complete game loop. I added some global variables to switch the game mode. And added `Exit()` to clean up resources because I use many steering object pointers.

#### Boid

#### Kinematic

#### SteeringComponent

#### SteeringObject

## Algorithms

#### Basic motion

![](/img/in-post/ai-write-up-01/1.JPG)

#### Seek

![](/img/in-post/ai-write-up-01/1.gif)

#### Wander

![](/img/in-post/ai-write-up-01/2.gif)

#### Flocking

![](/img/in-post/ai-write-up-01/3.gif)




