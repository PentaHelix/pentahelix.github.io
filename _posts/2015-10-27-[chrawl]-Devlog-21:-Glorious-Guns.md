---
published: false
---

A lot of guns, a lot of Systems

<!--excerpt-->

Hey everybody, this week I'll explain how gun-generation works in chrawl. As a note, I have not yet finished GunGen, however most of the key systems are in place and it's mostly down to content now.


##Gun Generation

Every Gun is built up from 5 "modules". These modules are *body*, *barrel*, *magazine*, *optics 1*, and *optics 2*. Personally, I like to use [Blender](http://www.blender.org) for modelling, however this should work with most modelling programs. My poor sculpting skills are one of the reasons why I chose to render my game in ASCII, so get used to lots of programmer art. Here's an example of a gun module:

![Gun Mesh](http://imgur.com/N4pgWzs.png)

This is just the basic mesh, a pretty generic gun body. The object has 4 empty transform children. (In Blender they are called plain axis empties, the only important thing is that they have a position and a rotation.) These transforms are the points on which the other modules will attach on. 

![Attachment Points](http://imgur.com/84mNUh4.png)

Finally, I unwrap the mesh onto a empty UV with a simple pattern which will later be filled by the texture generator. 

![UV](http://imgur.com/PypsH3i.png)

Here are some other modules:
![Barrel](http://imgur.com/oU5FYxP.png)
![Magazine](http://imgur.com/L7pUySr.png)

Once the models are finished I import them into Unity. There are no prefabs for modules. GunGen.cs simply instantiates from .fbx files.

```c#
public class GunGen{
	public static GameObject MakeGun(int r, int b, int m){
		<Instantiate Modules>
		<Create Empty GameObject to serve as parent>
        <Align attachment points using Align()>
		<Setup various Components>
        <Add procgen Texture>
        <Return gameobject>
    }
}
```

The parameters r, b, and m are the ID's for rifle(=body), barrel, and magazine. 
Align is a very simple method that positions/rotates the parents of two transforms so that they align.

```c#
static void Align(Transform t1, Transform t2){
	t1.parent.rotation *= (Quaternion.Inverse(t1.rotation) * t2.rotation);
	Vector3 pos = t2.position - t1.position;
	t1.parent.position += pos;
 }
```

##Texture Generation

##Where to go from here
The thing I am working on right now is module properties, which is going to be a json that lists certain properties for every module, such as tags, custom textures, ... . This makes it possible to have a module requiere certain tags from other modules to create more fine-tuned module synergies. 

I also want to expand the modularity of this system, e.g. separate the gun body into more modules. The number of module permutations grows exponentially to the number of modules per gun, so this would increase the gun variety drastically.

Any other improvements/suggestions? Leave a comment and let me know!

See you in a fortnight
~Jakob
