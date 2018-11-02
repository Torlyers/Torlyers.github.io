> Terrain Builder & Random Generation

## Summary
Some sandbox game like Minecraft has a large and complex terrain. If there is a terrain system in the game, it will be very convenient to build a game environment. I like Minecraft very much, so maybe the terrain is voxel-based. I watched a tutorial in GDC this year, and I think using perlin noise to generate voxel-based terrain generation is durable in three weeks. I'm not sure if I can finish what I want to do but I think I can learn a lot in the process and finish at least basic terrain generation algorithm.

## Features

#### Customize voxel cubes

Allow users to design and customize voxel cubes and set generation  probability of each kind of cubes. In this way, the system is more flexible and user can adjust the resources distribution. It means there will be human-readable parameter data files.

#### Unity Style Terrain Builder

Allow users use mouse to build terrain height map. Users can use brush to "print" mountains and lakes in a plane. After building, users can export terrain files.

#### Automatically Terrain Generation

If users doesn't want to manually "print" a map, they can also choose to automatically generate terrain, and export it.

## Challenges

#### Terrain Generation Algorithm
The main challenge is the how to generate terrain. Now I'm thinking about using perlin noise and voxel to do it. And I won't add textures or some other thins to make it too complex. I will do more research on it.

#### Data File Format Design
What does the terrain data file look like is another question. I think I can figure it out after I can generate terrain. The problem here is that I need to make both human-readable files and binary files. Although we have already made it twice, but I think this time it will be more complex and difficult.

#### User Interface Design
About this problem, I want to make it as easy as possible. Maybe I won't make a GUI and let user use command line to do it. But at least they can see the render result.