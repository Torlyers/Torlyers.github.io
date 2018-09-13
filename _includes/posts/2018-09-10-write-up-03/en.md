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

```c++
PlatformHelper::ClearColorBuffer(cos(time), cos(time), sin(time));
```
![](/img/in-post/write-up-03/background.JPG)


#### Effect

***Initialization***

```c++
effect.Initialize("data/Shaders/Vertex/standard.shader", "data/Shaders/Fragment/test_fs.shader");
```

***Memory***
![](/img/in-post/write-up-03/GLEffect.JPG)
![](/img/in-post/write-up-03/D3DEffect.JPG)

#### Mesh

***Indice***

***Initialization***
```c++
mesh.Initialize(vertices, indices);
```

***Memory***
![](/img/in-post/write-up-03/GLMesh.JPG)
![](/img/in-post/write-up-03/D3DMesh.JPG)

## Discussion

## Traps and Tips
* Strongly recommend comparing tool in visual codes. Save your eyes, save your time!
![](/img/in-post/write-up-03/CodeCompare.JPG)

---

## Appendix

[Click to download the Game](/assets/A03_Zhitao.zip)
