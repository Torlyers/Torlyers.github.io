> Maya Plug-in Mesh Exporter

## The game
[Click to download the Game](/assets/A07_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

Press `W`, `A`, `S`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the gameobject on the left, and to move the gameobject on the right if `Shift` is pressed.

Press `Num1` and `Num2` to switch the mesh of the gameobject on the left.

## Summary
After build human-readable mesh files, we want to apply it into more uses. This time we build meshes in Maya and export it into our own formats. Literually, we can export whatever meshes to the game engine only if it has less than 65535 vertices, which is enough for most of the meshes that we want to render in real-time. In this way, we don't need to calculate a cube's vertices and indices anymore :)

## Requirements
[Assignment Requirement](/assets/Requirement_07.pdf)

***[Game Gif]***
![](/img/in-post/write-up-06/1.gif)

#### MayaMeshExporter

The `MayaMeshExporter` depends on `Windows` project, and no project depends on it. It's just an exporter, which is not used in the engine or the game. 

#### Vertex Data

There is unused data in Maya's mesh file. I chose to ingore it and only take position data. I know at least we can use color information. I didn't do that just because I was lazy and I'll add color to my meshes this weekend.

#### Debugging

#### Mesh with Too Many Vertices
Since we use `uint16_t` to save our indice data, we cannot have over 2^16 vertices in each mesh. Generally we won't use mesh with so many vertices in a game, so I choose to check the number of vertices before import and reject it if a mesh have too many vertices.

## Traps and Tips
* 

---

## Appendix

[Click to download the Game](/assets/A07_Zhitao.zip)
