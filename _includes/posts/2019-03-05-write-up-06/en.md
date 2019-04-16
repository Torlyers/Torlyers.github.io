> Texture

## The game
[Click to download the Game](/assets/GA06_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## Summary

This time we added textures to the game. For build pipeline, I added `TextureBuilder` and changed material part to add diffuse texture to materials. UV information was added to vertex structure. For run time, I added codes to load and bind textures and sampler states, which have responding buffer in GPU.

## Requirements

#### Build Pipeline

***UVs***

To use texture, we need to have UV information in mesh. While exporting mesh file, we need to add UV position to every vertex. The coordinate in D3D is different with that in Maya, which is a left-hand coordinate. You can see the difference below. Respoinding changes should be made in mesh builder, andvertex structure.

![](/img/in-post/write-up-gra-06/1.png)

***Material***

We need to add texture file path to material. Here's my readable material file.

```lua
{
	EffectName="data/effects/standardAlpha.eft";
	Color={200, 35, 35, 255};
	DiffuseTexturePath="data/textures/man.tex";
}
```

Diffuse texture path is an optional value. If user doesn't want it, the engine will use a default one, which is included in engine contents and will always be built.

There may be some other textures in the future like normal map. This item can be a list.

#### Load & Bind

While loading material, we'll also load a texture and get a handle. Before rendering, we need to bind texture to a texture buffer according to an register id, which is already defined in `shader.inc`, making sure every shader has it. 

Same thing should be done for sampler state. But i didn't add it to every texture. I just created a default one while initialization and bind it. All textures will use the same one.

#### Shader

To use texture, we need to update vertex layout description in C++ code, and vertex shader input layout in shader code. Every vertex has a new element UV, whose semantic is `TEXCOORD`. After we got UV information in vertex shader, we can pass it to fragment shader and call
```HLSL
SampleTexture2d(g_diffuse_texture, g_diffuse_samplerState, i_uv)
```
to get a `float4` color. And then we can blend it which whatever color we like.

#### Result

![](/img/in-post/write-up-gra-06/1.JPG)

