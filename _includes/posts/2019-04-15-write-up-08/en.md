> Filtering, Mipmaps, and Alpha Channel

## The game
[Click to download the Game](/assets/GA08_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 


## Normal
To add lighting to meshes, we need to add normal infomation to vertices. Maya can help do that and export the readable file we need. We finally use normals in the fragment shader (or you can use it in vertex shader but it's not accurate enough). Therefore we need to pass normals to shader.

To do this, we need to add it to vertex class, vertex input layout, and shader input. Use `NORMAL` as semantic. 

## Diffuse Light


## Ambient Light


## Result

![](/img/in-post/write-up-gra-08/1.gif)
