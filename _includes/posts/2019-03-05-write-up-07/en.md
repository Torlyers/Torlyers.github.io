> Filtering, Mipmaps, and Alpha Channel

## The game
[Click to download the Game](/assets/GA06_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## Requirements

#### Filtering

While sampling texels from tertures, different filtering method can have different results. The following images shows the difference when camera is close to the texture.

![](/img/in-post/write-up-gra-07/1.jpg)

While bilinear filtering is disabled, fragment uses point sampling, which means the color of the pixel is the color of the nearest texel. In this way, you can see the texel block while camera is close. 

Bilinear filtering samples five nearest texels near the pixel and get linear average color of these texels so that there are some difference between adjecent pixels, which makes the final image has smoother transition.

#### Mipmaps

![](/img/in-post/write-up-gra-07/1.gif)

We don't always need full resolution for all textures beacuse sometimes textures are far away from camera. While compiling, we can generate a series of pictures with different resolution for one texture. And we can use low resolution ones while camera is far. This technique is called mipmaps.
Using mipmaps can avoid ailasing. And lower resolution images cost less in run-time. 


![](/img/in-post/write-up-gra-07/4.gif)
![](/img/in-post/write-up-gra-07/5.gif)

The image has eight levels of mipmaps from 400x400 to 1x1. First gif shows textures without mipmaps and the second gif uses mipmaps. We can see that using mipmaps can provide better visual performance

#### Alpha Channel

![](/img/in-post/write-up-gra-07/2.jpg)

While alpha blending is disabled, we can discard some pixels according to its alpha value and show more details. The method is to set a threshold and discard all pixels whose alpha values are lower than it. We can see that the texture on the right has more clear border.

#### Texture Scrolling

![](/img/in-post/write-up-gra-07/3.gif)

To implement this effect, we need to translate the coordinate of the texture and set sampler state to clamp. In this way, texture will be duplicated while its position is out of range. One important thing is to make sure the texture is  continuous.

#### More Texture Effect

![](/img/in-post/write-up-gra-07/2.gif)

This effect is also to change the coordinate of texture. The v value is scaled while u value is translated.