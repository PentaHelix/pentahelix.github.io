---
published: false
---

Hey everyone to the first devlog for chrawl2 (that's what it's called for now, incredibly creative, I know). Since I've spent the past two weeks re-implementing many games, I started with my favorites (which are ProcGen Libs). So far I've got mesh generation and map generation working, a basic implementation of gun generation, and some texture generation. I'll focus on the Mesh Gen in this devlog.

I use procedural Meshes primarily for building up the map since I want it to be dynamic and also efficient. In chrawl1 I used to construct a map by placing a 30 x 40 grid of blocks and empty spaces. This worked for some time, but later when I added shops and such I had to awkwardly resize and move some blocks, it got relly messy. So for chrawl2 I wanted to go for something more "lightweight" and simple. For this nwe approch I am generating a list of rooms and paths connecting them and then creating custom meshes to build the walls of them.

```
	
```