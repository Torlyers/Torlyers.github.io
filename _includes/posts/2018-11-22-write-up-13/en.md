> Terrain System

## Summary
Terrain system was finished this week. It provides generate heightmap and responding colored terrain mesh. The heightmap project used perlin noise to make the terrain consistent and smooth. Users can change some parameters in user setting file to customize their terrain meshes. After building terrain meshes, users can use my terrain class or their own mesh class to import terrain files.

![](/img/in-post/write-up-13/7.JPG)

## Features & Tutorial

Before using my projects, you must make sure that:

* Your engine supports 32-bit mesh.

There are three projects you need to use:

* Terrain (under `Engine` folder)
  
  output: static lib
  references: Graphics, Math, Platform, Results
* TerrainBuilder (under `Tools` folder)

  output: static lib
  referances: AssetBbuildLibrary, HeightMapGenerator, Mcpp, Platform, Windows
* HeightmapGenerator (under `Tools` folder)

  output: exe
  references: Graphics, Math, Platform, UserInput, Windows

You need to add `terrain` asset type to `AssetBuildFunctions.lua`
```lua
NewAssetTypeInfo( "terrains",
	{
		ConvertSourceRelativePathToBuiltRelativePath = function( i_sourceRelativePath )
			-- Change the source file extension to the binary version
			local relativeDirectory, file = i_sourceRelativePath:match( "(.-)([^/\\]+)$" )
			local fileName, extensionWithPeriod = file:match( "([^%.]+)(.*)" )
			return relativeDirectory .. fileName .. ".ter"
		end,
		GetBuilderRelativePath = function()
			return "TerrainBuilder.exe"
		end,	
	}
)
```
And add a new table in `AssetsToBuild.lua`
```lua
terrains = 
{
	"HeightMaps/heightmap500.raw",
},
```

#### Generate Heightmap
To make a terrain mesh, a heightmap is needed. Heightmap is an image file of `raw` format. It must be a squared 8-bit monochrome image, in which every pixel is present by a byte.

There are two ways I recommend to generate a standard heightmap. First is to use terrain component in Unity engine. The advantage is that users can paint terrains in any ways they like. While exporting a `.raw` file, just remember to set its resolution and save it in 8-bit.

![](/img/in-post/write-up-13/2.JPG)

The other one is to use my terrain generator to randomly generate a heightmap. It's easy to use, just set the size of it in user setting file and run `HeightmapGenerator.exe`. A random heightmap will be generatored in the folder you set. here is a heightmap it generated.

![](/img/in-post/write-up-13/4.JPG)

To use the executable file, you need to run CMD in its output path like this:

![](/img/in-post/write-up-13/1.png)


#### Generate Terrain Mesh
After you add heightmap files' paths into `AssetsToBuild.lua` file and build `TerrainBuilder` project, terrain files will be automatically generated in `data/HeightMaps` with a `.ter` extention. Then you can use these binary files. One importang thing is that the size of your maps must be the same with the `MapSize` parameter in user setting file, or you'll get a wrong mesh.

To avoid confliction, I have a `cTerrain` class and responding effect and shader files. Basically, it is a gameobject class, and shader just output the input color. So it's fine to use your own classes and files.

My terrain binary file has data like this:

* `uint32_t` vertexCount
* `uint32_t` indexCount
* `Vertex[]` vertexBuffer
* `uint16_t[]` or `uint32_t[]` indexBuffer

`Vertex` is like that:
```C++
struct Vertex
{
	float   position[3] = {0, 0, 0};//x, y, z
	uint8_t color[4] = {255, 255, 255, 255};//r, g, b, a
};
```

#### User Settings
Here is my user setting file called `terrainSettings.lua`. Put it in `MyGame\Contents\HeightMaps` so that it can be read automatally.
```lua
    --Heightmap Basic Settings   
    MapSize = 300;  --The image size will be Mapsize * Mapsize
    VertexScale = 0.05; --The distance between two vertices
    HeightScale = 5.0; --The real height of a vertex will be (0, 1) * HeightScale
    RelativeOutputPath = "../../../../MyGame_/Content/HeightMaps/";
    Extention = ".raw";

    --Heightmap Level Color Settings
    Levels=
    {
        --Height [0, 255], Color[0, 255]
        {Name = "SeaLevel", Height = 10, Color = { 71, 163, 201, 255, }, },
        {Name = "LandLevel", Height = 80, Color = { 118, 95, 62, 255, }, },
        {Name = "ForestLevel", Height = 120, Color = { 132, 196, 85, 255, }, },
        {Name = "SnowLevel", Height = 180, Color = { 255, 255, 255, 255, }, },
    };

    --Perlin Noise Settings
    Persistence = 0.1;
    Frequency = 0.1;
    Amplitude = 200.0;
    Octaves = 4;
```
The basic settings are easy to understand and I add comments, which mainly effect the terrain's size.
Setting level colors is a cool feature I think of the system. It allow you to add colors you like to the terrain. The color of a vertex depends on its height. You can add as many colors to it as you like.
The last part is about perlin noise. Generally you don't need to change that. However you can try to change that to get different terrain styles.


## Traps & Tips
* If you already pass correct buffers to GPU, you still need to draw   responding amount of vertex while rendering.
  ```c++
  glDrawElements(mode, static_cast<GLsizei>(m_indexCount), intType,   offset);
  ```
  Here I used to pass `m_vertexCount` to it, which is much smaller then index count. Finally, the terrain mesh is incomplete drew.

* Perlin noise generated random float numbers, I have to interpolate them into [0, 255] area to use them.

* If you want to use UserSetting project in compiling period, you need to change the working folder because it's different with that in running time.


## Appendix
[Click to download the demo](/assets/TerrainDemo.zip)
[Click to download projects](/assets/TerrainProjects.zip)