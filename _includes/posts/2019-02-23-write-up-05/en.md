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


***Texture***

***UVs***

***Material***
```lua
{
	EffectName="data/effects/standardAlpha.eft";
	Color={200, 35, 35, 255};
	DiffuseTexturePath="data/textures/man.tex";
}
```

#### Load & Bind



#### Shader

#### Result

![](/img/in-post/write-up-gra-06/1.JPG)

