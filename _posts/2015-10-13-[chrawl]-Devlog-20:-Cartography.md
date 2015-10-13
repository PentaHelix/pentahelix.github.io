---
published: false
---

In this week's devlog we will look into generating a procedural map.

<!--excerpt-->

Here is what a standard map in chrawl2 looks like:
![Map Side]()

As you can see, there are different rooms, with paths coneccting them. Rooms spawn at different heights, however no rooms will ever be on top of each other, which makes colission checking as well as a minimap easier. 

![Map Top]()

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

At the start of generating a map, one room is placed at (0,0). All other rooms are placed according to this algorithm:

1. Select random room
2. Select random direction d(N,E,S,W)
3. Select random length l
4. Starting from the chosen room, move l in direction d
5. Place new room with random w/h at that point
6. Check new room for overlaps
7. Room has no overlaps:
	Add room to room array, goto to 1. if more rooms need to be generated, else quit
   Room has overlaps
    Scrap room, go to 1


To explain this method we have to look at how rooms are built after they have been generated.
![Room Exits]()

To actual method for building rooms is too long for a blog post so instead, let me write it up in pseudocode

```
foreach(i in w*2+h*2){ //Once around the room
	if(room.exits.Contains(i)){
    	continue;
    }else{
    	BuildWall(i);
    }
}
```

This is very simplified, but the basic principle is the same. It loops around the room and places a wall if no path exits the room at that segment.

One such loop (there are 4, one for each side) looks like this:
```
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

This uses MeshGen, which we looked at last DevLog. 