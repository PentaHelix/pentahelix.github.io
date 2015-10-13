---
published: true
layout: post
---


In this week's devlog we will look into generating a procedural map.

<!--excerpt-->

Here is what a standard map in chrawl2 looks like:
![Map Side](http://imgur.com/jUma1Xa.png)

As you can see, there are different rooms, with paths connecting them. Rooms spawn at different heights, however no rooms will ever be on top of each other, which makes collision checking and minimaps easier. 

![Map Top](http://imgur.com/R7vjCM6.png)

When generating the map, a room is represented as an object with certain properties:

```c#
public class Structure{
	public int x;
	public int y;
	public int w;
	public int h;
	public int z;
	public List<int> exits = new List<int>();
	public Structure(int x, int y, int w, int h, int z){
		this.x = x;
		this.y = y;
		this.w = w;
		this.h = h;
		this.z = z;
	}

	public bool Intersects(Structure s, bool margin=true){
		if(margin)return !(s.x > x + w || s.x + s.w < x || s.y > y + h || s.y + s.h < y);
		else return !(s.x >= x + w || s.x + s.w <= x || s.y >= y + h || s.y + s.h <= y);
	}
}

```

The properties *x,y,z*/*w,h* are position/size as you would expect. *exits* is a list of 
ints from which paths originate, but back to that later.

At the start of generating a map, one initial room is placed at (0,0). All other rooms are placed according to this algorithm:

1. Select random room
2. Select random direction d (N,E,S,W)
3. Select random length l
4. Starting from the chosen room, move l in direction d
5. Place new room with random w/h at that point
6. Check new room for overlaps
7. No Overlaps: Add room, create a Path between the two rooms, go to 1
8. Overlaps: Scrap room, go to 1

That's more or less the whole algorithm. However, up to now all we have is an array with objects representing the rooms, nothing is actually in the scene yet. This means we still have to make some quads and build the rooms.

After a certain amount of rooms have been placed, the algorithm stops. Now, a loop loops through every room and builds them up.

![Room Exits](http://imgur.com/lu3llzP.png)

First of all, the walls surrounding the room are placed. Walls may not be place in a spot where a path leads away for the room, so every index where a path was placed has been added to the *exits* List\. With this, it is easy to loop through all the segments and only place valid ones. 

```c#
foreach(i in w*2+h*2){ //Once around the room
	if(room.exits.Contains(i)){
    	continue;
    }else{
    	BuildWall(i);
    }
}

```

This is very simplified, but the basic principle is the same. It loops around the room and places a wall if no path exits the room at that segment.

One of these loops (there are 4, one for each side) looks like this:

```c#
for(int i = 0; i < s.w; i++){
		if(s.exits.Contains(i))continue;
		rend = MeshGen.MakeQuad(
			new Vector3[]{
				new Vector3(x+i*3,     z+3, y+h),
				new Vector3(x+(i+1)*3, z+3, y+h),
				new Vector3(x+(i+1)*3, z,   y+h),
				new Vector3(x+i*3,     z,   y+h)
			},matWall);
}

```

After creating all the walls, floor and ceiling are pretty easy to place.


If I had more time I would go more in depth on this, but alas, chrawl is calling :)

I hope you enjoyed this somewhat, see you in 2 weeks, where we will be looking at procedural textures and Guns!
