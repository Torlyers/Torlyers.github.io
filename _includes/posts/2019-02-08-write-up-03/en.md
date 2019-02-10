> Local-to-projected transform

## The game
[Click to download the Game](/assets/GA03_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the sphere. 


## Requirements

#### Local-to-projected transform

For every vertex, to get its position in projection space, we need to transfrom it from local space into projection space. Before we calculate it by transforming three times in vertex shader, which requires three matrix multiplying. However, we could calculate a local-to-projected matrix instead and save it in constant buffer per drawcall, so that we don't need to calculate in shader, which could be cheaper.

To get the local-to-projected matrix, we need to concatenate three metrix together. They are local-to-world, world-to-camera, and camera-to-projection. The last two are saved in constant buffer per frame so that I chose to concatenate them first and save it for every drawcall.

```c++
Math::cMatrix_transformation s_matrixWorldToProjection;
```


#### The number of instructions

Here are the difference between using local-to-projected matrix and not. It includes 2 `mov` instructions that aren't about matrix calculation.

```c++
// using
// Approximately 12 instruction slots used
```

```c++
// not using 
// Approximately 26 instruction slots used
```

We saved nearly a half instructions by simply adding a  local-to-projected matrix in constant buffer.

