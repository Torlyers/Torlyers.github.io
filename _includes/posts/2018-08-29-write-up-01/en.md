> Solution Configration & Customized Fragment Shader


## Summary
Assignment 01 is about how to use visual studio to manage a solution of a large size. Before doing this, I have some experience on VS project settings, however, I didn't really understand the process. Through this assignment, I understand how to handle refenences and dependencies between projects, how to use property sheets and macros to handle cross-platform problems. Another important thing is that I start to know the implementation of configration after modify `.vcxproj` and `.filters` files.


## References & Dependency
The thing I learned from creating Graphics project and finally successfully building it is that **If you don't realize it, you will make mistakes.**

I failed three times here because missed something. I think the key here is that a solution could include many projects and each project may depends on each other. To make the solution work, projects should be built in the correct order. Visual studio will build projects in order after manually setting project references. (But there is a time I need to set project dependency).

About the difference between **Reference** and **Dependency**, I think the former one is more about link, in other words, a project need to use codes in other projects. And the latter one is more on project layer. Some libraries need to be built first, or other projects cannot be built.

## Cross-Platform  

## Project Property Sheet
Setting project propeties is always a annoying thing before you really start to code. A good solution should be robust in different platforms and to do this, you need to add some macros. And you want to make your file structure clear, make input, output, and projects of differents seperate. VS default configration is not a good choice so that you need to modify the properties so many times.
Property sheet make it so

## Finally My Game
 



---

## Appendix

[Click to download the Game](/assets/A01_Zhitao.zip)
