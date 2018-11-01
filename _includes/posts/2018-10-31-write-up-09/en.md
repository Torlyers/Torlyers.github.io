> Effect Builder

## The game
[Click to download the Game](/assets/A09_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

Press `W`, `A`, `S`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the gameobject on the left, and to move the gameobject on the right if `Shift` is pressed.

Press `Num1` and `Num2` to switch the mesh of the gameobject on the left.

## Summary
This time we do the same things to effect assets as mesh assets, creating human-readable `.lua` files, adding `EffectBuilder` project to build them into binary files, and extracting data to initialize effect objects.

## Requirements
[Assignment Requirement](/assets/Requirement_09.pdf)

***[Game Image]***

![](/img/in-post/write-up-09/1.JPG)

#### Human-readable Effect File

![](/img/in-post/write-up-09/2.JPG)

In this file, the first two items are relative paths of vertex shader and fragment shaders of the effect. The following three items are render settings that are save in `cRenderState` as a `uint8_t` intager. Obviously, using bits to save bools is not easily readable, so I chose to use simple bool varibles to save them.   


#### Binary Effect File

![](/img/in-post/write-up-09/3.JPG)

I save the length of a path in the front of it. I think the last path doesn't need to save a length. I'm thinking if we have much more paths in the future, we can use loop to read them. In this case, add one more length intager costs not too much. So I keep it to make them in the same order. And I put render state bits in the end.

#### Extracting

Here's the code I used to extract data from binary files.

```C++
auto currentOffset = reinterpret_cast<uintptr_t>(dataFromFile.data);
uint8_t vertexPathLength = *reinterpret_cast<uint8_t*>(currentOffset);

currentOffset += sizeof(uint8_t);
char* vertexPath = reinterpret_cast<char*>(currentOffset);

currentOffset += vertexPathLength;
uint8_t fragmentPathLength = *reinterpret_cast<uint8_t*>(currentOffset);

currentOffset += sizeof(uint8_t);
char* fragmentPath = reinterpret_cast<char*>(currentOffset);

currentOffset += fragmentPathLength;
uint8_t renderStateBits = *reinterpret_cast<uint8_t*>(currentOffset);
```

## Traps and Tips


---

## Appendix

[Click to download the Game](/assets/A09_Zhitao.zip)
