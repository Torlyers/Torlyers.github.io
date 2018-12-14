> Minecraft Flight Simulator

## The game
[Click to download the Game](/assets/FinalProject.zip)

#### Controls

Use mouse to control direction.

Press `SPACE` key to accelerate.

## Summary


## Game Features

***[Game Image]***

![](/img/in-post/write-up-15/1.JPG)

* A 500x500 voxel terrain that has different kinds of cubes (land, forest, and sea). 
* Player can fly over the terrain with a first person controller. 
* Player can collect coins in the game world.
* The terrain is drawn in both triangle mode and line mode.

## Terrain System

Now I calculate all vertices according to meightmaps. There are two ways I'm trying to convert every vertex into a cube. First is doing it in compiling time, for every vertex, I calculate all 8 vertices and 36 indices in building time. The advantage is we only need to draw one mesh when rendering. The disadvantage is the binary file is very big (23.6mb), and it isn't interactable. The other method is to draw many cubes in positions of vertices, which will have too many draw calls. Now I'm prefering the second method and trying to find some ways to reduce draw calls.

## Collision Detect System

To detect overlap and collisions, I chose to use Yitong Dai's collision system.

[EAE6320 3D Collision Response](https://yzd0014.wixsite.com/dyt1205/blog/eae6320-3d-collision-response)

