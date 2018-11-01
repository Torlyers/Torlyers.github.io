> Effect Builder

## The game
[Click to download the Game](/assets/A09_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

Press `W`, `A`, `S`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the gameobject on the left, and to move the gameobject on the right if `Shift` is pressed.

Press `Num1` and `Num2` to switch the mesh of the gameobject on the left.

## Summary
Binary files are much smaller and faster than reading lua files. So this time we build human-readable mesh files into binary files. In this case, the game doesn't need to interact with Lua stack in run-time at all, but read data from binary files directly. Now we have not only human-readable files for designers and artists, but also binary files which is efficient.

## Requirements
[Assignment Requirement](/assets/Requirement_09.pdf)

***[Game Image]***

![](/img/in-post/write-up-09/1.JPG)

#### Human-readable Effect File

![](/img/in-post/write-up-09/2.JPG)


#### Binary Effect File

![](/img/in-post/write-up-09/3.JPG)

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

## Traps and Tips
* I tried to convert buffers into STL `vector` so that we don't need to pass four parameters, and STL does have a constructor that doesn't copy the buffer. However, it doesn't work in OpenGL. 

---

## Appendix

[Click to download the Game](/assets/A09_Zhitao.zip)
