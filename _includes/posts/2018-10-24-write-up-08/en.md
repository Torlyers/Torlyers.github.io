> Build Binary Mesh Files for Run-time

## The game
[Click to download the Game](/assets/A08_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

Press `W`, `A`, `S`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the gameobject on the left, and to move the gameobject on the right if `Shift` is pressed.

Press `Num1` and `Num2` to switch the mesh of the gameobject on the left.

## Summary
Binary files are much smaller and faster than reading lua files. So this time we build human-readable mesh files into binary files. In this case, the game doesn't need to interact with Lua stack in run-time at all, but read data from binary files directly. Now we have not only human-readable files for designers and artists, but also binary files which is efficient.

## Requirements
[Assignment Requirement](/assets/Requirement_08.pdf)

***[Game Image]***

![](/img/in-post/write-up-08/1.JPG)

#### Binary Files

Here is an example of my binary files. The first part is vertex count, which is underlined by blue in the first image. The following is index count. The next part in vertex buffer. And the last part underlined by red in the second image is index buffer.
I chose to put two counts in the front because I need one of them to calculate the size of the responding buffer, so that I can get the offset to calculate the position of the next buffer.

![](/img/in-post/write-up-08/2.JPG)
![](/img/in-post/write-up-08/3.JPG)

#### Platforms

I think we do need to build different binary files for OpenGL and D3D. The reason is they have different index order while rendering. If we have the same human-readable file, we need to modify it for one platform at run-time. Before, we do that while reading mesh data. However, if we continue it, it will waste lots of time and it's hard to swap elements in a data chuck. 

#### Extracting

Here's the code I used to extract data from binary files.

```C++
auto currentOffset = reinterpret_cast<uintptr_t>(dataFromFile.data);
uint16_t vertexCount = *reinterpret_cast<uint16_t*>(currentOffset);

currentOffset += sizeof(vertexCount);
uint16_t indexCount = *reinterpret_cast<uint16_t*>(currentOffset);

currentOffset += sizeof(indexCount);
Vertex* vertexBuffer = reinterpret_cast<Vertex*>(currentOffset);
	
currentOffset += sizeof(Vertex) * vertexCount;
uint16_t* indexBuffer = reinterpret_cast<uint16_t*>(currentOffset);
```

#### Profiling

The two main advantages of binary files are that they are much smaller and faster. I believe the general solution of handling game data is build human-readable files into binary files. To prove this, here's the profiling data.

I build a donut that has 6429 vertices. The size of exported Lua file is 2.54mb. After building it to binary files, it's only 476kb. To read Lua file, it takes 0.297s. And it takes 0.001s to read the binary file.


## Traps and Tips
* I tried to convert buffers into STL `vector` so that we don't need to pass four parameters, and STL does have a constructor that doesn't copy the buffer. However, it doesn't work in OpenGL. 

---

## Appendix

[Click to download the Game](/assets/A08_Zhitao.zip)
