> Filtering, Mipmaps, and Alpha Channel

## The game
[Click to download the Game](/assets/GA08_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 


## Normal
To add lighting to meshes, we need to add normal information to vertices. Maya can help do that and export the readable file we need. We finally use normals in the fragment shader (or you can use it in vertex shader but it's not accurate enough). Therefore we need to pass normals to the shader.

To do this, we need to add it to vertex class, vertex input layout, and shader input. Use `NORMAL` as semantic. 

## Diffuse Light

To calculate diffuse light is easy. For a point on the mesh surface, it will be fully lit when the light direction is parallel with the normal. With the increase of the angle between the light direction vector and the normal vector, the light on this point will be darker. The formula is like this:
```c++
float3 diffuseColor = g_directionalLight_color * cosine;
```
The range of cosine here is [0, 1].

The diffuse light I used in my game is a directional light, which is a subclass of `GameObject`. While submitting, I will submit it with other objects so that I can draw it if we add a mesh to it. Also, I submit it's direction vector and color to constant buffer per frame so that they can be used to calculate lighting. Since I already added Euler angles to rigidbody. I can get the direction vector from rigidbody directly.

Another important thing is that you must make sure that the light vectors and normal vectors must be in the same space. I chose to calculate them in world space. It's easy to understand and I do not need to transform light vectors. What I need to do is to output `o_normal_world` to fragment shader.

## Ambient Light

Ambient light is much easier. Literally, it is just a constant value that will be added to fragment color. I also made a derived class for it and added a color member variable to it. While submitting, I only need to submit the color to the constant buffer per-frame.

## Result

![](/img/in-post/write-up-gra-08/1.gif)
