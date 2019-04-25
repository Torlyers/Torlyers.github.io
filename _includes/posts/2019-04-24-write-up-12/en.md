> UI sprites

## The game
[Click to download the Game](/assets/GA10_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## Normal Map

In a mesh, every vertex has its own normal vector. For low-poly model, if we want more details, we can use normal maps to let every fragment has different normal vector. In this way, even a flat face can have bump details. 

Using normal map doesn't cost much because we just pass some normal vectors(normal map) to shader. The lighting model is still the same.

#### Build Pipeline

Building normal maps is similiar as building diffuse textures. But we want the normal textures are buildt with linear values because the "color" in normal maps is not actual color but normal vectors we want to use directly.

```c++
if (textureType == Graphics::TextureTypes::Normal)
	{
		sourceImage.OverrideFormat(RemoveSRGB(sourceImage.GetMetadata().format));
	}
```
To use normal maps in shader, we also need to add tangent and bitangent vectors to meshes. We can get them from Maya. In my vertex input layout, I added semantics `TANGENT` and `BITANGENT`.

#### Calculate Normal

The calculation is in shaders. First, we sample the normal map and get a vector in the range[0, 1]. We need to interpolate it to the range[-1, 1]. This result is not the real normal vector of the fragment. 

Second, we use tangent, bitangent, and normal to construct a `TBN` matrix, which is used to transfrom the normal offset to the final normal vector of the fragment.

```c++
const float3x3 TBNMatrix = float3x3(
		i_tangent_world.x, i_bitangent_world.x, i_normal_world.x,
		i_tangent_world.y, i_bitangent_world.y, i_normal_world.y,
		i_tangent_world.z, i_bitangent_world.z, i_normal_world.z );
```

Then we can use the normal we get to do lighting calculation.

#### Results

![](/img/in-post/write-up-gra-12/2.JPG)

![](/img/in-post/write-up-gra-12/6.gif)

![](/img/in-post/write-up-gra-12/1.JPG)

![](/img/in-post/write-up-gra-12/5.gif)

![](/img/in-post/write-up-gra-12/3.gif)
