> Cubemap

## The game
[Click to download the Game](/assets/GA10_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## Cubemap

Before we use 2D textures and use uv position to sample. A cubemap is basically a texture that contains 6 individual 2D textures that each form one side of a cube: a textured cube. The advantage of cubemap is that we can use a 3D vector such as direction vector to sample and we can use any tricks we know in shaders to get this vector.

![](/img/in-post/write-up-gra-14/1.png)

Building a cube texture is basically the same as other kinds of textures.
In my game, I only used a single cube texture for making a skybox and doing environment mapping.

#### Skybox

Skybox is a big cube that will move with the camera. In fragment shader, I used the local position (which is equal to direction vector) of each fragment to sample the cubemap.

```
o_color = SampleTextureCube(g_cubemap_texture, g_samplerState, i_samplePosition);
```

![](/img/in-post/write-up-gra-14/1.gif)
![](/img/in-post/write-up-gra-14/2.gif)

#### Environment Map

By using cubemap, we can make many interesting effects. These effects are called environment mapping. The most popular one should be reflection.

![](/img/in-post/write-up-gra-14/2.png)

We use view vector and normal vector to calculate the reflection vector, and use the result to sample on the cubemap. Then we can get a environment lighting.

In my gold metal material, there's no diffuse light. Both specular and environment reflection are multiplied by a yellow color. And in my earth material, since the glossness of sea part is 256, it looks very smooth. The land part doesn't have any reflection.

![](/img/in-post/write-up-gra-14/1.JPG)
![](/img/in-post/write-up-gra-14/2.JPG)
![](/img/in-post/write-up-gra-14/4.JPG)
![](/img/in-post/write-up-gra-14/3.JPG)
