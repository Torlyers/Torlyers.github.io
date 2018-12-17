> Minecraft Flight Simulator

## The game
[Click to download the Game](/assets/FinalProject.zip)

#### Controls

Use mouse to control direction.

Press `SPACE` key to accelerate.

## Summary

I made a simple flight simulator game, in which you can control a small plane flying over a voxel terrain with color. And you can collect coins in the game world. 

I used my terrain system to generate voxel terrain and use Yitong Dai's collision detetion system to implement overlap detection between the plane and coins.


## Game Features

***[Game Image]***

![](/img/in-post/write-up-15/1.JPG)

* A 500x500 voxel terrain that has different kinds of cubes (land, forest, and sea). 
* Player can fly over the terrain with a first person controller. 
* Player can collect coins in the game world.
* The terrain is drawn in both triangle mode and line mode.

## Terrain System

Now I calculate all vertices according to meightmaps. There are two ways I'm trying to convert every vertex into a cube. First is doing it in compiling time, for every vertex, I calculate all 8 vertices and 36 indices in building time. The advantage is that we only need to draw one mesh when rendering. The disadvantage is the binary file is very big (56.4mb), and it isn't interactable.  

The other method is to draw many cubes in positions of vertices, which will have too many draw calls. I did try this method and I found that the render system cannot afford so many draw calls. In OpenGL, I could only draw 5000 cubes and 2000 cubes in Direct3D, which is obvious not enough because even a 100x100 terrain needs 10000 draw calls. 

Finally I had to choose the first method. It worked well. I implemented minecraft style terrain. The only problem is that I cannot add collider to the terrain. And I added border lines to it by drawing it twice. 

So my terrain system is still easy to use. You don't need add any codes to use it. Just use my heightmap generator to generate a random map and buiild it to a terrain mesh. Then you can use it.

## Collision Detect System

To detect overlap and collisions, I chose to use Yitong Dai's collision system.

[EAE6320 3D Collision Response](https://yzd0014.wixsite.com/dyt1205/blog/eae6320-3d-collision-response)

Yitong's implementation is pretty solid. It can detect both movement collision and rotation collision and reslove them perfectly. I intented to use his system to make colliders for my terrain. Because I failed to add so many cube gameobjects. I just added some coins.

About the interfaces. Yitong designed his interfaces based on his own gameobject class. And he added a AABB bounding box to his rigidbody class, which means I need to use his classes to use his physics system. But I still wanted to use my own classes. So I add some features required by his physics system to my gameobject class. Then I pushed all gameobejcts to a vector and passed it to physics system. And it can detect collision between these gameobjects.

## Some Other Updates
* Added eular angles to rigidbody to implement first person player controller.
* Added gameobject project and class. Added some virtual functions like `Update()` and `EventHit()`.

## What I Learned in this Class

I think the most important part I learned in this class is engine structure. Before I learned some engine features in Joe's class but I don't know how to connect them together. In this class, I learned how to design platform-independent interfaces, how to set vs projects. The engine framework JP provided has so many codes we can learn form like assetbuilder, graphics library. I think I know more about game engine structure now. For examplel, before when I was studying some OpenGL, I had no idea how to design class. Now I know we can have platform independent mesh class and effect class.

Another thing is coding skill. We did some low-level stuff this semester. We use raw pointers to handle data buffers. We consider much about memory. We use goto to handle returns. We tried to output all error messages. All these stuff I think will be very useful in the future.



