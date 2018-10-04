> Gameobject, Camera & Draw Call Cache

## The game
[Click to download the Game](/assets/A06_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

Press `W`, `A`, `S`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the gameobject on the left, and to move the gameobject on the right if `Shift` is pressed.

Press `Num1` and `Num2` to switch the mesh of the gameobject on the left.

## Summary
Campared with hard-coded vertices data, we prefer save data in files in specific format. This time we use lua script to save vertices and indices, and we want to make it human-readable so that we can modify and debug easily. There would be some time we need to look up in the data file directly, which means make data files readable (and we always want the data structure efficient) is necessary.

## Requirements
[Assignment Requirement](/assets/Requirement_06.pdf)

***[Game Gif]***
![](/img/in-post/write-up-06/1.gif)

#### Human-readable Asset File
It's not so easy to make data file readable because basically it's a text file. I consider that trying to make it like a e-table file could be better, so I use some comments as colume titles and index. 

For vertices data, I think they should be ordered. And for each vertex, it could include position, color, and different kinds of textures. In this case, we need to set keys for them.

For indices data, I just put all of them in a table, and use comments as index, which did't add any complexity but make it more readable


#### My Mesh File
```lua
return
{
	Vertices=
	{
	--Index-Position-----------------Color---------------------Texture-----------
		{--0
			Position={0.5,0.5,0.0},
		},
		{--1
			Position={0.5,-0.5,0.0},
		},
		{--2
			Position={-0.5,-0.5,0.0},
		},
		{--3
			Position={-0.5,0.5,0.0},
		},
	};

	Indices=
	{
		--0
			0, 3, 1,
		--1
			1, 3, 2,
	};
}
```

#### Mesh Builder & Mesh Manager
This time we just simply cope all the mesh files that were registered to the output folder. In the future we will transfer them into binary files.

`Mesh Manager` is a static variable which manages all the meshes that were loaded from files. We use handles to get the mesh we want, which is the same as how we manage shaders. The good thing is that we can use the same base builder and base manager to manage all the resource files.



## Traps and Tips
* While reading lua files, you need to pop after every pushing. The hard part is to keep the whole process clear, or it's difficult to debug

---

## Appendix

[Click to download the Game](/assets/A06_Zhitao.zip)
