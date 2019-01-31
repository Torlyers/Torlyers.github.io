> Steering Algorithms

## Control

* Press `1` to switch to the basic motion mode.
* Press `2` to switch to the seeking mode.
* Press `3` to switch to the wandering mode.
* Press `4` to switch to the flocking mode.
* Press `5` to use kinematic steering.
* Press `6` to use dynamic steering. 
* Press `7` to use switch leader. 

While seeking, press mouse left button to change the seeking target.

## Summary

This assignment includes two parts. The first part is to develop a game framework. I implemented game objects, kinematics, and steering components. They are well encapsulated and have user-friendly interfaces. The second part is steering algorithms. I implemented more than 10 steering algorithms based on physics and combined them into four motion modes.  

In the process, I thought a lot about algorithms and the framework. Some of the algorithms can have something wrong or can be improved. While blending steering outputs, I spent much time on how to set parameters. Another thing is to unify interfaces for all steering objects. it's hard to get a perfect code structure and outcome. I did my best and I think it works well.

After finished this assignment. I think the most important thing I learned is that Although these basic algorithms are not difficult, we can still get complex behaviors by blending them together like flocking and adjusting parameters. It could be very useful in a real game. Therefore, a structure that can easily blend basic behavior is essential for a steering system. And basic steering behaviors should be robust enough.

## Framework

#### Game

The game is based on `Openframeworks`, which has a complete game loop. I added some global variables to set positions, switch game mode for updating, and save game objects. I also added a virtual `Exit()` function to clean up resources because I use many steering object pointers.

#### Boid

Boid is derived from GameObject class, it has a kinematic component and a steering component. It's well encapsulated that can draw itself and breadcrumbs. It can call different steering objects by steering component and update kinematic component, which is thought is a good design.

#### Kinematic

About Kinematic class, I added some functions to make it better to use such as `Translate()`, `RotateRad()`, `GetDirection()` and so on. Compared with setting max acceleration and other physical parameters in steering objects, I think set it in kinematic is more reasonable. And you can simply do some restriction when update. If steering objects want to use it, they can simply get them by functions I provided. For updating, it's will use kinematic mode or dynamic mode according to current game mode.

#### SteeringComponent

In class, Rogelio said we need to write classes for every algorithm that use the same face `GetSteering()`. To do this, we need to pass parameters while constructing steering objects. However, these parameters are very different from each other. How to manage these objects are also a difficult question. 
Therefore, I made this intermediate class. It has pointers to all steering objects, all the parameters to initialize them, and necessary setters. It will construct all the steering objects in a correct turn. Also, to call these steering functions is pretty easy. The interface is like this:
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

In this mode, the boid just goes straight from bottom left in a fixed speed, and turn left when colliding with borders. 
```c++
output.velocity = boid->GetKinematic().GetDirection() * speed;
```
It's a kinematic movement and I set the "output" manually just for testing the kinematic component, which works well enough.
Although this part is simply, I think it important. You must make sure your kinematic is robust, or there will be bugs that are difficult to fix while using the output of steering algorithms because you don't know it's from the algorithm itself or kinematic. I made as many tests as I can here to make sure the kinematic and the boid work perfectly.

#### Seek

***Kinematic***

![](/img/in-post/ai-write-up-01/4.gif)

***Dynamic***

![](/img/in-post/ai-write-up-01/1.gif)

For the seek behavior, I used four steering algorithms:
* DynamicSeek
* KinematicArrive
* DynamicArrive
* DynamicAlign

`DynamicSeek` is for the basic movement before the boid reaches the slow radius. You can see the boid has smooth acceleration and keeps a fixed velocity according to the .gif. In this algorithm, the boid always gets a max acceleration, which I think is not necessary. The better way is to make the user set this parameter. So I wrote a setter for steering component to make it more flexible. 

`KinematicArrive` doesn't have a good effect because it simply stops the boid. In my experience, any sudden movements in a game look not natural. But if gameplay needs it, it can still of use.

`DynamicArrive` works well. However, you have to adjust acceleration and time to target a lot to make it stop in time. Literally, you can calculate the stopping time according to deceleration and radius. You cannot make sure the boid always stop because we don't change velocity directly. Then it will dangle between the target. So I manually stop the boid after it enters the target radius. If the velocity of the boid is small enough, it looks natural enough. There is a possible improvement in the algorithm. While calculating the factor, use this formula could be more accurate:
```c++
factor = (distance - targetRaduis) / (slowRadius - targetRadius);
```
Both arrive algorithm have a problem, which is they give the boid a fixed velocity/acceleration. I don't think it's reasonable. Seek steering can handle this. Arrive steering shouldn't do anything before starting arriving because the movement before could be complex. Here is my solution.
```c++
output = boid->GetSteeringComponent()->GetSteering(DYNAMIC_ARRIVE);
if (output.type == NONE)
    output = boid->GetSteeringComponent()->GetSteering(DYNAMIC_SEEK);
```

`DynamicAlign` is the only angular steering behavior in this part. Actually, I believe it should be called `DynamicAngularArrive` because it's algorithm is basically an angular version of `DynamicArrive`. Therefore, it also has the problems I mentioned about arrive. I tested max angular acceleration and time to target many times to make sure it has the best behavior.

#### Wander

***Kinematic***

![](/img/in-post/ai-write-up-01/5.gif)

***Dynamic***

![](/img/in-post/ai-write-up-01/2.gif)

I implemented both kinematic wander and dynamic wander. You can see the kinematic version is more linear while the dynamic version is more curved. But it's not because of the algorithm themselves. Parameters are the key elements here. The basic mentality of both algorithms is to let the boid keep making a small rotation. And they all use `RandomBinomial()` to get this small rotation. So their behaviors are pretty similar literally. My feeling is dynamic wandering is more smooth cause it uses angular acceleration to turn to new direction.

One important thing is that the result of `RandomBinomial` is often small. So the max angular velocity/acceleration should be large enough because the target changes every frame, or the wander will be totally linear. For the same reason, the parameter `targetDistance` shouldn't be too larger than `targetRadius` in `DynamicWander`.

`DynamicWander` is implemented by finding a random target and delegated to `DynamicSeek` and `DynamicAlign`, which reminds me of making basic behaviors solid is pretty important.


#### Flocking

![](/img/in-post/ai-write-up-01/7.gif)
![](/img/in-post/ai-write-up-01/8.gif)

The flocking behavior is different. Here I have a leader boid and a bunch of follower boids. The leader uses wander steering and followers use flocking steering. The flocking steering behavior is blended by these algorithms:
* DynamicSeperation
* DynamicSeek
* DynamicVelocityMatch
* DynamicAlign

`DynamicVelocityMatch` is used to keep boids following orderly. The weight of this behavior depends on if you want boids in order or not.

`DynamicSeperation` add "forces" from boids together and apply it to every boid. The possible problem here is that the final output is pretty small. Here are two reasons. First is that these accelerations have a different direction and will encounter each other. Second is that all the "forces" will be divided by the square of the distance, which could be a large number. Therefore, the `k` value should be large enough.

Here's the problem. While blending behaviors, some outputs are "naturally" if lower weight because of algorithm and parameters. I consider that we should make sure that all the outputs we want to blend should be distributed in the same range, or the weights are meaningless. It's difficult to change algorithms to make sure they have output in a similar range. Maybe we can interpolate them into a proper range while blending.

About the number of followers, I didn't find an obvious difference between small numbers and large numbers. You may notice that the distance between every boid is nearly the same. I think it's because of the separation behavior. The "force" between boids prevent them to stick together.

About switching leaders, it essentially just applies `DynamicWander` to a random follower, and make the elder leader be a follower. All the boids just follow a new leader and nothing else. One thing I noticed here is that the leader is often on the front because other boids depend on it. If you switch a boid as a leader, it will go into the front and other boids will follow him. Their kinematic won't change too much because their velocity is kind of the same.


## Appendix

[Click to download the Game](/assets/AIAssignment.zip)