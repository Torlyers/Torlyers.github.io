> UI sprites

## The game
[Click to download the Game](/assets/GA10_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## Sprite & Sprite Mesh

Before we have `Gameobject` class. It has a mesh handle and a material handle. Submitting them to the render library then we can render a gameobject. In my perspective, sprite is a special gameobject. All sprites use a same mesh and same shaders. So I add a new mesh class that is just for sprites.

#### Sprite
The sprite class is like gameobject class. I got rid of mesh handle. One thing I changed is that I used a static vector to save all sprites and provide a `CreateSprite` function. All sprites will be automatically added to the vector. In this way, user don't need to submit them manually.

#### Mesh
All sprites use the same mesh, a quad. the vextex can be hardcoded in the engine directly. One sprite vertex only has two elements.

```c++
struct SpriteVertex
{
    int8_t  position[2] = { 0, 0 }; //x, y

    uint8_t uv[2] = { 0, 0 }; //u, v
    
};
```

The values of positions and UVs will only be 0, 1 or -1 because There are only four vertices. In the input layout, the format of positions is `DXGI_FORMAT_R8G8_SNORM` while the format of UVs is `DXGI_FORMAT_R8G8_UNORM`. When pass vertex buffer to the vextex shader, both positions and UVs will be normalized.

While drawing, we don't need index buffer here. So we need to call `Draw()` rather than `DrawIndexed()`. The primitive should be set to `D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP`. Because all sprites use the same mesh, so we only need to set vertex buffer and primitive once.

## Constant Buffer

I didn't make a different constant buffer for sprite. but there's a little difference. I didn't save a transformation matrix in sprite class. While submitting, the user only needs to submit a material handle and the position to the render library. 

we can use position vector to make a transformation matrix by putting it to the last column of the matrix. 
```c++
m_03( i_translation.x ), m_13( i_translation.y ), m_23( i_translation.z )
```
And in the vertex shader, we don't really need to use the whole matrix to transfrom. We can only take the translation from the matrix like this:
```HLSL
g_transform_localToWorld[3].xyz
```
This is the world position of the sprite. For every vertex, its world position equals to local position add the translation. 

## Render Command

Before we have two types of render commands: independent and dependent. Now we have the third one: sprites. So we use 2 bits to save types. Because sprites are always on the top layer, so we render them the last. Sprite's rendercommand contains effect index, material index, and drawcall index.

## Effect & Shaders

We don't need to enable depth testing, depth writing, and back or front culling. We do need to enable alpha blending.

The UI sprites will always on the same positions of the screen no matter how you rotate the camera, which means you don't need transform vertex position to view space. We only need to transform it to the world space and do some scale as we like. In fragment shader, we just need to set diffuse texture and material color.

## Result

![](/img/in-post/write-up-gra-10/1.gif)

I only set the sprite mesh's vertex buffer and primitive type once at the beginning of decoding and rendering sprite's render commands so that we can reduce some D3D api calls.

![](/img/in-post/write-up-gra-10/1.JPG)
