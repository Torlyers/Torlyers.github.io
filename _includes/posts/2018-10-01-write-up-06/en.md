> Gameobject, Camera & Draw Call Cache

## The game
[Click to download the Game](/assets/A06_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

Press `W`, `A`, `a`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the gameobject on the left, and to move the gameobject on the right if `Shift` is pressed.

Press `Num1` and `Num2` to switch the mesh of the gameobject on the left.

## Summary
In this assignment, I implemented Gameobject class in the easiest way like this.

## Requirements
[Assignment Requirement](/assets/Requirement_06.pdf)

***[Game Gif]***
![](/img/in-post/write-up-06/1.gif)

#### Human-readable Asset File
In this assignment, I implemented Gameobject class in the easiest way like this.


#### My Mesh File
```Lua
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

## Traps and Tips
* Have fun!

---

## Appendix

[Click to download the Game](/assets/A06_Zhitao.zip)
