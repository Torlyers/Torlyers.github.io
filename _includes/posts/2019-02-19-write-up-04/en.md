> Material

## The game
[Click to download the Game](/assets/GA04_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## Requirements

#### Material

***Class***

`Material` is a new kind of asset that is managed by asset manager. It consists a handle of effect and a `float[4]` array of color. By adding `Load()` function, the material manager can hold all material handles. To get the handle of effect, calling `Load()` function of `Effect` class is necessary.
  
***Human-readable File***

My material's .lua file is like this
```lua
{
    EffectName="data/effects/standard.eft";
    Color={35, 125, 200, 255};
}
```
Here the color property is optional. If user didn't add any color. `MaterialBuilder` will add a default color for this material.
  
***Binary File***

![](/img/in-post/write-up-gra-04/2.JPG)

Here I use `uint_8` to save colors to save a few bytes. They will be convert into float while loading.

***Constant Buffer***

To pass material data to GPU, we need to create a new constant buffer called per material. It only has color data now. 
```c++
struct sPerMaterial
{
	float color[4];
}
```

#### Rendering

***Interface***
  
Now we submit mesh and material rather than mesh and effect. A `GameObject` holds a mesh handle and a material handle. We can get handle of effect easily from material while we need it.

***Sorting***

Campared with before, we should consider material while sorting. The index of material should be put behind effect index. The following are mesh index, depth, and renderer index infomation that saves matrix. In this way, we'll draw meshes according to effect first, then accirding to material from near to far.
  
![](/img/in-post/write-up-gra-04/1.gif)


#### Debugging

For meshes using different materials. we need to bind different constant buffer. You can see instructions in pink. The last draw call didn't bind constant buffer per material.

![](/img/in-post/write-up-gra-04/1.JPG)