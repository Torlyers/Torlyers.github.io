> Pass Data from Game to Engine

## The game
[Click to download the Game](/assets/A05_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

Press `Left` key to hide an object.

Press `Right` key to change the effect of the other object.

## Summary
This time we moved data from graphics library to the game application. In this way, we make the game and the engine 

## Requirements
[Assignment Requirement](/assets/Requirement_05.pdf)

***Game Gif***
![](/img/in-post/write-up-04/gamerun.gif)


#### Pass Data from Game to Graphic Library

***Background Color***

This is how we change background color in the game. If we pass parameters rely on time, we can make background color animations. The alpha value has a default value as 1.0f. It should be called in `SubmitDataToBeRendered()` function.
```c++
eae6320::Graphics::SetBackgroundColor((float)sin(time), 0.5f, (float)cos(time));
```

***Mesh & Effect***

We pass two pointers of mesh and effect to graphic library. Literally we could pass any combinations. It should be called in `SubmitDataToBeRendered()` function.

```c++
eae6320::Graphics::SubmitMeshAndEffect(m_mesh1, m_effect1);
```

***Two Buckets***

In most of the games (or engine), different systems refresh in different frequency like physics and render. In another words, they have different loop threads. The update time of two systems is hardly the same so that there are two data "buckets", the render system updates after a bucket is full of data.

#### Memory Debugging
I stored mesh and effect data is a fixed-sized array and use a pointer pointing to it.

***OpenGL***
![](/img/in-post/write-up-04/glSize.JPG)
What we add to `Mesh` and `Effect` is a reference number which is of 2 bytes. It can be put into the empty alignment space. So they are still of 16 bytes. And `sDataRequiredToRenderAFrame` includes a `ConstantBufferFormat::sPerFrame`, a pointer, a float[4] array, and a `uint8_t` counter, which is 168 bytes in all.

***Direct3D***
![](/img/in-post/write-up-04/d3dSize.JPG)
In Direct3D, the reference count varable can be put into the empty alignment space, so that `Mesh` is still 32 bytes and `Effect` is still 48 bytes. the data structure of render data is nearly the same with OpenGL but the pointer is of 8 bytes and aligned by 8 bytes, which is tatolly of 176 bytes.

## Traps and Tips
* Need to be very careful to think about where to use `IncrementRefenenceCount()` and `DecrementRefenenceCount()`.


---

## Appendix

[Click to download the Game](/assets/A05_Zhitao.zip)
