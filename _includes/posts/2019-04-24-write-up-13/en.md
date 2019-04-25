> PBR & Specular map

## The game
[Click to download the Game](/assets/GA10_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## PBR

Physically based rendering (PBR) is a rendering way to simulate the real world according to physics theory. Three conditions of PBR are:
* Microfacets
* Energy Conservation
* BRDF
 
In microfacets theory, any surfaces can consist of many micro specular faces. When microfacets are in order, the surface is smooth. And reflection light is more specular. We can use a roughness value to describe the surface.

![](/img/in-post/write-up-gra-13/1.png)

In energy conservation theory, light can be reflected or refracted. But neither of them can contain more energy than the incoming light. Some materials, such as metals, will absorb all refraction light. And reflection light should be dimmer than the incoming light.

#### BRDF

The reflectance equation to calculate PBR lighting is like this: 

![](/img/in-post/write-up-gra-13/1.JPG)

This is a integral equation that is suitable for non-point light source. $L_o$ is the output light. $L_i$ is the input light. $f_r$ is the BRDF function. $n$ is the normal vector of the fragment. When light source is only a point. The function can be simplefied:

![](/img/in-post/write-up-gra-13/2.JPG)

The BRDF function we used is *Cook-Torrance* function. It has two parts: diffuse and specular.

![](/img/in-post/write-up-gra-13/3.JPG)

In diffuse function, $c$ is the color of original surface color. And in specular function, we have three different functions to calculate different parts of the surface reflection.

![](/img/in-post/write-up-gra-13/4.JPG)

Substitute these three functions into specular function, we can reduce some factors and get the final result to calculate specular light.

#### Specular

Specular exponent from 16 to 128.

![](/img/in-post/write-up-gra-13/10.jpg)

## Specular Map

We just saw how roughness($α$) affact the specular lighting. For one object, there may be roughness difference. In this case we can use specular map so that every fragment can have different roughness. I used `DXGI_FORMAT_BC4_UNORM` to save the texture file.

specular map is a single channel texture. I use black to represent rough and white to represent smooth. The range of each pixel is [0, 1], and I extrapolate it to [0, 256] is shader.

![](/img/in-post/write-up-gra-13/4.gif)
