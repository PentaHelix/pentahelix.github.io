---
published: true
---

Hey everyone to the first devlog for chrawl2. Since I've spent the past two weeks re-implementing many games, I started with my favorites (which are ProcGen Libs). So far I've got mesh generation and map generation working, a basic implementation of gun generation, and some texture generation. I'll focus on the Mesh Gen in this devlog.

I use procedural Meshes primarily for building up the map since I want it to be dynamic and also efficient. In chrawl1 I used to construct a map by placing a 30 x 40 grid of blocks and empty spaces. This worked for some time, but later when I added shops and such I had to awkwardly resize and move some blocks, it got relly messy. 

![Bad Method](http://i.imgur.com/XLOZtqS.png)

So for chrawl2 I wanted to go for something more "lightweight" and simple. For this new approch I am generating a list of rooms and paths connecting them and then creating custom meshes to build the walls of them, creating only 1 Quad per wall.

![Good Method](http://i.imgur.com/dEYBxK4.png)

```c#
public static GameObject MakeMesh(Vector3[] verts, int[] tris, Vector3 pos){
    	GameObject g = new GameObject("Mesh");
    	MeshFilter rend = g.AddComponent<MeshFilter>();
    	Mesh m = new Mesh();
    	rend.mesh = m;
    	m.vertices = verts;
    	m.triangles = tris;
    	m.uv = new Vector2[]{new Vector2(0,1), new Vector2(1,1), new Vector2(1,0), new Vector2(0,0)};
    	g.transform.position = pos;
    	return g;
    }
```

This method is in MeshGen.cs and creates a Mesh given vertices and triangles, attaches it to a GameObject and returns it. 

Additionally, I have a utility method for creating a quad from four Vector3's in global space.

```c#
public static MeshRenderer MakeQuad(Vector3[] corners, Material mat = null){
    	Vector3 pos = (corners[0] + corners[1] + corners[2] + corners[3])/4;
    	Vector3[] verts = new Vector3[]{corners[0]-pos,corners[1]-pos,corners[2]-pos,corners[3]-pos};

		int[] tris = new int[6]{0,1,3,1,2,3};

		GameObject g = MakeMesh(verts,tris,pos);
    	MeshRenderer r = g.AddComponent<MeshRenderer>();
    	g.AddComponent<MeshCollider>();
    	g.isStatic = true;
    	if(mat != null)r.material = mat;
    	return r;
    }
```
(pos is the centerpoint of the quad, since Meshes shouldn't have their center at (0/0/0) and verts way off in the distance)

That's basically it, the quads also need to be adjusted for culled faces, but I'll cover that in the Map Gen Devlog.