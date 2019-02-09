> Render Command

## The game
[Click to download the Game](/assets/GA01_Zhitao.zip)

#### Controls

Use mouse to control direction.

Press `W`, `A`, `S`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the sphere. 

## Summary

Before this assignment, we need to bind a effect before every drawcall, which is too expensive. This time we use render commands to save all the data needed for a draw call. We can sort all render commands before render a frame so that meshes using the same effect can be rendered together. Also, meshes are sorted by the distance from the camera, which makes sure that we can always render the mesh that is closer to the camera first.

## Requirements

#### Handles and Indices

I used pointers to save meshes and effects in game objects before, which works well. However, we need to encode them into render cammands this time, handle index (a small intager) is more suitable than a pointer (an address). So I replaces all the pointers in game objects and renderers with handles.

While submitting data for rendering, I just submit indices of mesh an effect. By calling manager, I can easily get pointers to them, which is very convenient. In this way, what I only need to do is to encode depth and indices of mesh, effect, and constant buffer per drawcall into a render command.

#### Encode Render Command

I used a `uint64_t` variable as render command. It concludes for parts in the following order: effect handle index, mesh handle index, constant buffer per drawcall index, and the distance between mesh and camera. Each part occupies 16 bits. effect index and distance really matter in sorting. First, you need to group all meshes with same effect together. Second you want to render the most closest mesh first. In this case, sorting all rendermands from small to large, then you can get the order you want.

I use bit operator `|` and `<<` to implement encoding while use `&` and `>>` to implement decoding. Besides, I defined some macros such as `MESHBITSNUM` to avoid magic numbers.  


#### Result

Now we have a render command for every drawcall, what we is to do is just sort it from small to large before render a frame. By decoding the render command and call `cManager`, we can get all the data we need.

Here is the render timeline:

![](/img/in-post/write-up-gra-01/1.JPG)

Here is the render order:

![](/img/in-post/write-up-gra-01/1.gif)

![](/img/in-post/write-up-gra-01/2.gif)

We can see that for one effect, it calls two `DrawIndexed`. And according to the .gif, it always renders the nearest mesh in all meshes using the same effect.

