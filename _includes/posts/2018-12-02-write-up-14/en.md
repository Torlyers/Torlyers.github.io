> Minecraft

## Summary

For the final project, basically, I planed to make a simple Minecraft game. The game world has a big random terrain made of cubes. Player can walk on the ground. Besides, there are some gameobjects on the ground that the player can interact with. Players can create and destroy cubes they selected.

This week I was trying to transfer my terrain mesh into a Minecraft-style voxel terrain. The basic method is to place a cube on every vertex's position, which is easy to understand. However, there were some problems while I really developing it.

Besides, I adjusted my first person camera. Add eular angles and fix the problem that mouse cursor blocked by screen borders.

![](/img/in-post/write-up-14/2.JPG)

## Game Features

![](/img/in-post/write-up-14/1.png)

* A 500x500 voxel terrain that has different kinds of cubes (land, forest, and sea). Player can walk on the terrain with a first person controller. Also, player can switch to flying mode.
* There will be some interactable gameobjects (some other kinds of cubes). Player can create and destory cubes.
* The terrain will only render faces that exposed to air.

## Terrain System

Now I calculate all vertices according to meightmaps. There are two ways I'm trying to convert every vertex into a cube. First is doing it in compiling time, for every vertex, I calculate all 8 vertices and 36 indices in building time. The advantage is we only need to draw one mesh when rendering. The disadvantage is the binary file is very big (23.6mb), and it isn't interactable. The other method is to draw many cubes in positions of vertices, which will have too many draw calls. Now I'm prefering the second method and trying to find some ways to reduce draw calls.

## Collision Detect System

To detect overlap and collisions, I chose to use Yitong's collision system.

![EAE6320 3D Collision Response](https://yzd0014.wixsite.com/dyt1205/blog/eae6320-3d-collision-response)


The good thing is that my gameobjects are all in a fixed size, so that I don't need to set collider's size for every of them. And I can use the `Gameobject` class that Yitong provides. 