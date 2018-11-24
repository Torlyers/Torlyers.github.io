> Terrain System

## Summary
Terrain system was finished this week. It provides generate heightmap and responding colored terrain mesh. The heightmap project used perlin noise to make the terrain consistent and smooth. Users can change some parameters in user setting file to customize their terrain meshes. After building terrain meshes, users can use my terrain class or their own mesh class to import terrain files.

![](/img/in-post/write-up-13/7.JPG)

## Features

#### Generate Heightmap
To make a terrain mesh, a heightmap is needed. Heightmap is an image file of `raw` format. It must be a squared 8-bit monochrome image, in which every pixel is present by a byte.

There are two ways I recommend to generate a standard heightmap. First is to use terrain component in Unity engine. The advantage is that users can paint terrains in any ways they like. While exporting a `.raw` file, just remember to set its resolution and save it in 8-bit.

![](/img/in-post/write-up-13/2.JPG)

The other one is to use my terrain generator to randomly generate a heightmap. It's easy to use, just set the size of it in user setting file and run `HeightmapGenerator.exe`. A random heightmap will be generatored in the folder you set. here is a heightmap it generated.

![](/img/in-post/write-up-13/4.JPG)

#### Generate Terrain Mesh

#### Set Colors

#### User Settings



## How to Use

Before using my project to use terrains, you must make sure that:

* Your engine support 32-bit mesh

There are three projects you need to use:

* Terrain (under `Engine` folder)
  
  There is a 

* TerrainBuilder (under `Tools` folder)
  
  aaaa

* HeightmapGenerator (under `Tools` folder)
  
  aaaa

## Traps & Tips

## Appendix
[Click to download the demo](/assets/TerrainDemo.zip)
[Click to download projects](/assets/TerrainProjects.zip)