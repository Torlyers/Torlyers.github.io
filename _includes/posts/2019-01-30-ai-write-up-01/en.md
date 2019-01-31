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

In the process, I thought a lot about algorithms and the framework. Some of the algorithms can have something wrong or can be improved. While blending steeing outputs, I spent much time on how to set parameters. Another thing is to unify interfaces for all steering objects. it's hard to get a perfect code structure and outcome. I did my best and I think it works well.

## Framework

#### Game

The game is based on `Openframeworks`, which has a complete game loop. I added some global variables to set positions, switch game mode for updating, and save gameobjects. I also added a virtual `Exit()` function to clean up resources because I use many steering object pointers.

#### Boid

Boid is derived from GameObject class, it has a kinematic component and a steering component. It's well encaspulated that can draw itself and breadcrumbs. It can calling different steering objects by steering component and update kinematic component, which is think is a good design.

#### Kinematic

About Kinematic class, I added some functions to make it better to use such as `Translate()`, `RotateRad()`, `GetDirection()` and so on. Compared with setting max acceleration and other physics parameters in steering objects, I think set it in kinematic is more reasonable. And you can simply do some restriction when update. If steering objects want to use it, they can simply get them by functions I provided. For updating, it's will use kinematic mode or dynamic mode according to current gam mode.

#### SteeringComponent

In class, Rogelio said we need to write classes for every algorithm that use the same face `GetSteering()`. To do this, we need to pass parameters while constructing steering objects. However, these parameters are very different from each other. How to manage these object is also a difficult question. 
Therefore, I made this intermidiate class. It has pointers to all steering objects, all the parameters to initialize them, and necessary setters. It will construct all the steering objects in a correct turn. Also, to call these steering functions is pretty easy. The interface is like this:
```c++
output = boid->GetSteeringComponent()->GetSteering(KINEMATIC_WANDER);
```
It will call the steering function according to the enum variable.

#### SteeringObject

SteeringObject is a pure virtual class. It's the base class of all steering behavior class. All of them must implement this function:
```
virtual SteeringOutput GetSteering() = 0;
```
About the return value `SteeringOutput`, it only has linear and angular in Rogelio's version, which I think is kind of vague. I made it universal for both kinematic and dynamic and add a type variable. In this way, I can use it for either kind of algorithms and be clear with its type.

## Algorithms

#### Basic motion

![](/img/in-post/ai-write-up-01/1.JPG)

#### Seek

![](/img/in-post/ai-write-up-01/1.gif)

#### Wander

![](/img/in-post/ai-write-up-01/2.gif)

#### Flocking

![](/img/in-post/ai-write-up-01/3.gif)

## Appendix

[Click to download the Game](/assets/A09_Zhitao.zip)


