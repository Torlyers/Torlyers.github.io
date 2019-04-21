> Color Space

## The game
[Click to download the Game](/assets/GA09_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

## SRGB Color Space

Color space is a color range that describes how color data is showed on physical equipment. The same color can be saved in different data formats and values in different space.

Before we calculate colors in linear color space and also output them in linear space. However, sRGB color space is a more popular one that is wildly used in many output areas such as monitors. sRGB is popular because of some historical reasons so we have to fit the standard, render colors in sRGB color space.

## Implementation

Telling graphics hardware to convert the linear values output into sRGB values is easy. We just need to set the format of swap chain description to `DXGI_FORMAT_R8G8B8A8_UNORM_SRGB`. It also means the final output in fragment shader should be in linear space.

In our engine, we should calculate all colors in linear space. However, texture files are saved in sRGB color space because they are made in some art programs such as Photoshop, which uses sRGB color space as default. So we need to transfer them into linear space in the texture builder. All other color settings such as background color, light colors, material color, mesh color should also be transferred into linear space and then be calculated.

The formula to transfer color from sRGB space to linear space
is:

![](/img/in-post/write-up-gra-09/1.JPG)

## Result

You can see the boundary between lit part and dark part is more obvious.

***Before***

![](/img/in-post/write-up-gra-09/2.JPG)

***After***

![](/img/in-post/write-up-gra-09/1.gif)
