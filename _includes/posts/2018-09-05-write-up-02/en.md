> Platform Independent Interfaces & GPU Debugging

[Click to download the Game](/assets/A02_Zhitao.zip)
## Summary

In this assignment, I have a basic understanding about the rendering pipeline of both Direct3D and OpenGL. What we want to do is to develop a platform-independent render library. And we want to draw different meshes with different shaders. So the first step is add platform-independent mesh class and effect class and write good interfaces, then implement in both platforms.

## Requirements
[Assignment Requirement](/assets/Requirement_02.pdf)

### Mesh
Mesh class is for models. I add a `Vertex` struct and a vector to save vertices. Now a vertex only contains position information. Since we will read `.obj` or `.fbx` files in the future, the vertex should contains more data like texture information and the mesh should contain indices.
Here are the interfaces in `cMesh.h` file, and they were implemented in two different `.cpp` files.

#### Interfaces
```c++
cResult Initialize();
cResult CleanUp();
void	Render();
```

### Effect
Effect class is for shaders. I'm not sure if "Effect" is the best name but I can't find a better one. It is mainly responsible for binding shaders. We may want use different shader files so that I add another constructor.
```c++
Effect(std::string i_VertexShaderPath, std::string i_FragmentShaderPath);
```
Same as the Mesh class, interfaces were implemented in two `.cpp` files.
#### Interfaces
```c++
cResult Initialize();
cResult CleanUp();
void    Bind();
```

### Use Interfaces
Create two instances first.
```c++
eae6320::Graphics::Effect effect;
eae6320::Graphics::Mesh mesh;
```
Then call the functions wherever we need them.
```c++
//Initialization
auto result = Results::Success;
result = effect.Initialize();
result = mesh.Initialize();

//Bind shaders
effect.Bind();
//Render mesh
mesh.Render();

//Clean up
const auto localResult = mesh.CleanUp();
const auto localResult = effect.CleanUp();
```

### Add Triangles
Just add one more triangle to draw a rectangle. In this assignment we didn't use indices so that I just simple cope and paste another triangle. To draw a house, just add one more triangle. What you need to make sure is just where the vertice are and their draw index. Direct3D is clockwise and OpenGL is counter-clockwise.

![](/img/in-post/write-up-02/run.JPG)

![](/img/in-post/write-up-02/house.JPG)

### GPU Debugging

We use VS Graphics Analyzer to debugging Direct3D and use Render Doc to debugging OpenGL. Press `PrtScn` button on the keyboard and either debugger can generate a report file.

#### Direct3D
![](/img/in-post/write-up-02/vsreport.JPG)

![](/img/in-post/write-up-02/vsdiagnose.JPG)


#### OpenGL
![](/img/in-post/write-up-02/rd.JPG)

![](/img/in-post/write-up-02/rdtv.JPG)

## Discussion
This is the very first step we try to develop a platform-independent render library. Once you have some basic knowledge about the render pipeline, the framework will be clear, and we'll know how to design classes, each of which is responsible for different funcitons. We add mesh and effect class this time, and we may add camera, light, skybox, and so on in the future. 

## Traps and Tips
* Debugging setting of a project is saved in the `.user` file.
* The axis in OpenGL and Dirext3D is different (Too well-known to be ignored).

---

## Appendix

[Click to download the Game](/assets/A02_Zhitao.zip)
