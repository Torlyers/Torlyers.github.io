> Platform Independent Render & Using Vertex Indices

## The game
[Click to download the Game](/assets/A03_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

## Summary
This assignment is to make the graphic library more platform-independent. Now we only have one `Graphics.cpp` file, and all platform-specific contents were moved to another place. In this case, if we want to modify the render process, we don't need to modify two different files.
Besides, hard-code mesh data and effect data are removed. Now both `Mesh` and `Effect` are data driven, and we can bind different shaders to differnt meshes, which is flexible enough.


## Requirements
[Assignment Requirement](/assets/Requirement_03.pdf)

***Game Gif***
![](/img/in-post/write-up-03/running.gif)

#### Platform-Independent Graphics.cpp
The key to finish the move is to find the differences between `Graphics.gl.cpp` and `Graphics.d3d.cpp`. Here are the differences. 

* In Direct3D, we need to init render target view and depth/stencil view, which doesn't exist in OpenGL.
* In render process, the interface of clearing color buffer is different.
* The interface of clearing depth buffer is different.
* Swapping buffer is different.
* And Direct3D views should be released in while cleaning up.

What I did was to move these codes into a differnt namespace called `PlatformHelper`, packing them by interfaces and call these interfaces. The implementations are platform-specific, but they are not in `Graphics.cpp` any more. Here are the interfaces.

```c++
cResult Initialize(const sInitializationParameters& i_initializationParameters);
void ClearColorBuffer(const float r, const float g, const float b, const float a = 1.0f);
void ClearDepthBuffer();
void SwapBuffer();
cResult CleanUp();
```
#### Background Color
Every frame, the color buffer will be cleared. And we can determine the color of the background. Both OpenGL and Direct3D accept 4 `float` parameters that representing RGBA color channels so that we can change these numbers to change the background color. If we can connect these numbers to system time, we can make an animate.

```c++
PlatformHelper::ClearColorBuffer(cos(time), cos(time), sin(time));
```
![](/img/in-post/write-up-03/background.JPG)


#### Effect

***Initialization***
The initialization of effect is pretty simple, the data we use here is vertex shader and fragment shader. So, we just need to use shader files' names to initialize an effect object.

```c++
effect.Initialize("data/Shaders/Vertex/standard.shader", "data/Shaders/Fragment/test_fs.shader");
```

***Memory***
In OpenGL, an effect object is 16 bytes, which includes 
In Direct3D, an effect object is 40 bytes, which indludes
***OpenGL***
![](/img/in-post/write-up-03/GLEffect.JPG)
***Direct3D***
![](/img/in-post/write-up-03/D3DEffect.JPG)

#### Mesh

***Indice***
To save both time and space, we use indices to present tertices that we need to render, which requires us to add another buffer and handler (or pointer). An important thing here is winding direction. To make sure same data works on both platforms, when we use the data in one platform, we need to swap the value of the second vertex and the third one in a triangle in the other platform.
```c++
swap(i_indiceData[i + 1], i_indiceData[i + 2]);
```  

***Initialization***
To initialize a mesh object, we just need to pass vertices buffer and indices buffer to it. 
```c++
mesh.Initialize(vertices, indices);
```

***Memory***
In OpenGL, an effect object is 16 bytes, which includes 
In Direct3D, an effect object is 40 bytes, which indludes

***OpenGL***
![](/img/in-post/write-up-03/GLMesh.JPG)
***Direct3D***
![](/img/in-post/write-up-03/D3DMesh.JPG)

## Traps and Tips
* Strongly recommend comparing tool in visual codes. Save your eyes, save your time!
![](/img/in-post/write-up-03/CodeCompare.JPG)

---

## Appendix

[Click to download the Game](/assets/A03_Zhitao.zip)
