> Gameobject, Camera & Draw Call Cache

## The game
[Click to download the Game](/assets/A06_Zhitao.zip)
#### Controls
Press `SPACE` key to slow down to 0.5 times, and release it to recover.

Press `W`, `A`, `a`, `D` to move the main camera. 

Press `I`, `J`, `K`, `L` to move the gameobject on the left, and to move the gameobject on the right if `Shift` is pressed.

Press `Num1` and `Num2` to switch the mesh of the gameobject on the left.

## Summary
Campared with hard-coded vertices data, we prefer save data in files in specific format. This time we use lua script to save vertices and indices, and we want to make it human-readable.

## Requirements
[Assignment Requirement](/assets/Requirement_06.pdf)

***[Game Gif]***
![](/img/in-post/write-up-06/1.gif)

#### Human-readable Asset File
It's not so easy to make data file readable. 


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

## Traps and Tips
* Have fun!

---

## Appendix

[Click to download the Game](/assets/A06_Zhitao.zip)
