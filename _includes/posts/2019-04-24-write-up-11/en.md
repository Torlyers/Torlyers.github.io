> Specular Light

## The game
[Click to download the Game](/assets/GA10_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## Specular Light

In this assignment, we add a Blinn-Phong specular lighting. Before we use ambiemt and diffuse lighting, which are not enough. We need specular lighting to show the smoothness of materials.

Specular lighting is based on the reflection of light. It depends on light direction, normal of surface, and view direction. In simple terms, when we look at the light reflection, we can see a specular.

![](/img/in-post/write-up-gra-11/1.png)

The formula to calculate specular in Blinn-Phong model is:

$Color = s * (N⋅H)^α$

In this formula, `s` is some some scalar or color value describing how reflective a material is. `α` is a exponent, which shows the smoothness of the material. `N` is the normal vector. `H` is the half vector of view vector and light vector.

We can see from the formula that the specular light is related to the camera. While the normal vector and half vector is closer, the light will be stronger. This is because if these two vectors are the same, the camera will be exactly on the reflection vector of the light. And this is the strongest specular color.

#### Result

***Light Direction***
![](/img/in-post/write-up-gra-11/1.gif)

***Camera Direction***
![](/img/in-post/write-up-gra-11/2.gif)

***Exponent***
![](/img/in-post/write-up-gra-11/1.JPG)

## Point Light

Point light is like a light bulb. It doesn't have a direction but a position, which is opposite to the direction light. And in the real world, a light bulb cannot light the whole world, so we need an attenuation value.
```c++
float attenuation = max(length(g_pointLight_position - positionWorld), 1);
```

![](/img/in-post/write-up-gra-11/3.gif)

