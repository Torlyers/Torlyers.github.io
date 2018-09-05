> Render Pipeline & GPU Debugging

[Click to download the Game](/assets/A01_Zhitao.zip)
## Summary
Assignment 01 is about how to use visual studio to manage a solution of a large size. Before doing this, I have some experience on VS project settings, however, I didn't really understand the process. Through this assignment, I understand how to handle refenences and dependencies between projects, how to use property sheets and macros to handle cross-platform problems. Another important thing is that I start to know the implementation of configration after modify `.vcxproj` and `.filters` files.


## References & Dependency
The thing I learned from creating `Graphics` project and finally successfully building it is that **If you don't realize what you are doing, you will make mistakes.** I failed two times here because of missing something. Once I missed some refenences of graphics project. Once my build order is not correct.

 I think the key here is that a solution could include many projects and each project may depends on each other. To make the solution work, projects should be built in the correct order. Visual studio will build projects in order after manually setting project references. (But there is a time I need to set project dependency).

About the difference between **Reference** and **Dependency**, I think the former one is more about link, in other words, a project need to use codes in other projects. And the latter one is more on project layer. Some libraries need to be built first, or other projects cannot be built.

## Cross-Platform  
We always want to use one interface to handle implements on different platforms like OpenGL and DirectX3D. Before I used macros to conditionally compile, which is a general solution. Now I learned excluding files in different platforms and using different project property sheets to make it more convinent.

## Project Property Sheet
Setting project propeties is always a annoying thing before you really start to code. A good solution should be robust in different platforms and to do this, you need to add some macros. And you want to make your file structure clear, make input, output, and projects of differents seperate. VS default configration is not a good choice so that you need to modify the properties so many times.
[Project property sheet](https://msdn.microsoft.com/en-us/library/669zx6zc.aspx) make it easy to set repeated items for different projects. By using property sheet they can share same macros and platform settings. When we add a new project, we can simple use the sheet we added.

## Finally My Game
Technically I just copy the `ExampleGame` project and change its unique unid to create "my" game. And I do the same to the property sheet and `BuildMyGameAssets` project. Finally I set `MyGame` project as the default start project. It's easy to do all this, but after reading `.project` and `.sln` codes, I realized why we must do this and wouldn't miss anything.

Customized fragment shader is the asset I add to my game. To change the triangle's color.
```c++
float rValue = sin( g_elapsedSecondCount_simulationTime );
float gValue = cos( g_elapsedSecondCount_simulationTime );
float bValue = sin( g_elapsedSecondCount_simulationTime );
```
![](/img/in-post/write-up-01/game.JPG)

---

## Appendix

[Click to download the Game](/assets/A01_Zhitao.zip)
