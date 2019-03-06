> Transparenrt materials

## The game
[Click to download the Game](/assets/GA05_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## Summary

This time I add transparent material to the engine. Depth test and alpha blending need to be enabled. And we need new render command type and new sorting order. Transparent materials are regarded as dependent ones and should be rendered from far to near at last.

## Requirements

#### Dependent Material

For transparent materials, we need to look through them and see other meshes behind, which means transparent materials depend on opaque materials. 

***Render States***

To enable alpha transparent effect, we need to enable depth test and alpha blend in render states settings.
```lua
{
	VertexShaderFileName="data/shaders/vertex/standard.shader";
	FragmentShaderFileName="data/shaders/fragment/standard.shader";
	EnableAlphaTransparency=true;
	EnableDepthBuffering=true;
	EnableDrawBothTriangleSides=false;
}
```
My `cRenderState` class is now handled by asset manager. For every draw call, if it uses the same render states as the last one, we don't need reset them.

#### Sorting

![](/img/in-post/write-up-gra-05/1.gif)

***Command Type***

Now we have dependent materials and independent materials. They have different sorting order. We use the first bit in render command to present these two type. Indenpendent material is 0 while dependent material is 1, which make sure that independent materials will be drawn first.

***Depth***

For dependent materials, depth must be considered first and sorted from far to near. We still want to sort all draw calls in one sorting. So I handled depth like this.
```c++
if (isEnableAlpha)
{
	depth = cameraFar - distance;
}
```
In this way, far objects will have prior rendering orders.