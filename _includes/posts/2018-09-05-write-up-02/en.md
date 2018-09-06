> Render Pipeline & GPU Debugging

[Click to download the Game](/assets/A02_Zhitao.zip)
## Summary

In this assignment, I have a basic understanding about the rendering pipeline of both Direct3D and OpenGL. 

## Requirements
[Assignment Requirement](/assets/Requirement_02.pdf)

### Mesh
Mesh class is for models. This time the data structure is pretty simple. A vertex only contains position information. Since we will read `.obj` or `.fbx` files in the future, the vertex should contains more like index and texture information. 
```c++
cResult Initialize();
cResult CleanUp();
void	Render();
```

### Effect
Effect class is for shaders. I'm not sure if "Effect" is the best name but I can't find a better one.
```c++
cResult Initialize();
cResult CleanUp();
void    Bind();
```

### Add Triagnles
Just add one more triangle to draw a rectangle. In this assignment we didn't use indices so that I just simple cope and paste another triangle. To draw a house, just add one more triangle.

![](/img/in-post/write-up-02/run.JPG)
![](/img/in-post/write-up-02/house.JPG)




### GPU Debugging

We use VS Graphics Analyzer to debugging Direct3D and use Render Doc to debugging OpenGL.

#### Direct3D
![](/img/in-post/write-up-02/vsreport.JPG)
![](/img/in-post/write-up-02/vsdiagnose.JPG)


#### OpenGL
![](/img/in-post/write-up-02/rd.JPG)
![](/img/in-post/write-up-02/rdtv.JPG)



## Discussion

## Traps and Tips
* Debugging setting of a project is saved in the `.user` file.
* The axis in OpenGL and Dirext3D is different (It's too well-known to be ignored).

---

## Appendix

[Click to download the Game](/assets/A02_Zhitao.zip)
