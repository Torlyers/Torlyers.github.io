> Generate Height Map

## Summary

This week I implemented most part of `TerrainBuilder`, and using Perlin Noise to generate a fixed-sized height map. In another word, the basic features of a terrain system are implemented. However, there are still some bugs and problems, and I also need to make it more user-friendly and efficient.

## Mesh From Height Map

To build a mesh from a height map is easy. Use x and y as a vertex's x position and z position, and use the value of a point as y position. Here is my implementation.
```c++
BYTE* data = reinterpret_cast<BYTE*>(dataFromFile.data);
for (int y = 0; y < MAP_SIZE; ++y)//row
{
	for (int x = 0; x < MAP_SIZE; ++x)//column
	{
		//calculate vertex
		Vertex v;
		v.position[0] = x * VERTEX_SCALE;//x
		v.position[2] = y * VERTEX_SCALE;//z
		v.position[1] = (float)data[x * MAP_SIZE + y] * HEIGHT_SCALE;//y
		vertexBuffer[x * MAP_SIZE + y] = v;
......
```
Here are some problems. If a height map has many vertices, you may not want to use all of them, we can ignore some points. I pretend to add a `STEP_LENGTH` macro here and allow user to set it. Also, I may need to support 32-bit vertex indices to allow users to use higher resolutions. Now it only support small height maps. 

![](/img/in-post/write-up-12/1.JPG)

## Perlin Noise
Now users can use `GenerateHeightMap()` function to generate a random height map (of a fixed size). For every pixel, you use `Perlin(int x, int y)` to calculate a random value as its height. 
```c++
for (int i = 0; i < MAP_SIZE; ++i)
{
	for (int j = 0; j < MAP_SIZE; ++j)
	{
		auto height = eae6320::HeightMap::Perlin((float)j, (float)i);
		heights.push_back((uint8_t)(height * 100.0f));
	}
}
```
You can find how to implement a decent Perlin noise function online(or source code) easily. But I do spend some time to learn its theory so that I can set parameters properly and choose more efficient random generator and interpolate algorithm like cosine interpolate. I will continue working on it.

![](/img/in-post/write-up-12/2.JPG)

## Challenges
I think I finished main features of the system, though it looks awful now. I'm confident I can make it better because I've figured unknown part out, and I have experience on the last parts, which I think will takes much time this week.

#### Vertex Count
I need to finish the optional challenge of using 32-bit vertex indices first, and try to find a way to reduce vertex count of terrain meshes.

#### Visual Effect
As you can see, it's hard to tell the "terrain" in the window becuase it just a white area. There are couple methods to solve it. But considering we don't have light and texture, I think adding color to it is a good solution.

#### Interface And User Settings
This part is the most difficult one. First, I need to implement a user setting file just like that of `Application` project. Second, I still want to all users to "paint" terrains themselves. I have an idea but I'm not sure if I have time to do it. But the good thing is we have lots of methods to generate meight maps.