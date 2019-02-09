> Render Command

## The game
[Click to download the Game](/assets/GA02_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the sphere. 

## Summary

This time I implemented four kinds of shader effects. Most of them were implemented implemented in fragment shader while shrink effect was implemented in vertex shader.

## Requirements

#### Color Pattern in Local Space

This effect is based on mesh's local position. To implement this effect, I calculated cos(x), cos(y), cos(z) as r, g, b. I passed it's local position to fragment shader. Another way is to pass constant buffer per drawcall to fragment shader but it's more expensive.

![](/img/in-post/write-up-gra-02/1.gif)

#### Color Pattern in World Space

It's nearly the same as the last effect. The only difference is to change local position to world position. Since we can access to `LocalToWorld` matrix in fragment shader, it's easy to calculate it's world position by `mul` function.

![](/img/in-post/write-up-gra-02/2.gif)

#### Shrink

Shrink is easier to implement. We only need the mesh to scale with the time. I made a scale matrix like this:
```c++
float4x4 scaleMatrix = float4x4(scale, 0, 0, 0,
             					0, scale, 0, 0,
						        0, 0, scale, 0,
						        0, 0, 0, 1.0);
```
The constant scale depends on system time. Use the matrix to transform the local position.

![](/img/in-post/write-up-gra-02/3.gif)

#### Change Color with Camera

The color of mesh will change according to the distance between mesh and camera. I added camera position to constant buffer per frame so that I can calculate the distance. In general, the mesh is white. While the camera move to the mesh, it turns to red and keeps red when the camera is near to it. 

![](/img/in-post/write-up-gra-02/4.gif)