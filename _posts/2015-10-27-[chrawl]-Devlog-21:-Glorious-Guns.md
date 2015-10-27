---
published: false
---

A lot of guns, a lot of Systems

<!--excerpt-->

Hey everybody, this week I'll explain how gun-generation works in chrawl. As a note, I have not yet finished GunGen, however most of the key systems are in place and it's mostly down to content now.

Every Gun is built up from 5 "modules". These modules are *body*, *barrel*, *magazine*, *optics 1*, and *optics 2*. Personally, I like to use [Blender](http://www.blender.org) for modelling, however this should work with most modelling programs. My poor sculpting skills are one of the reasons why I chose to render my game in ASCCI, so get used to lots of programmer art. Here's an example of a gun module:

![Gun Mesh]()

This is just the basic mesh, a pretty generic gun body. The object has 4 empty transform children. (In Blender they are called plain axis empties, the only important thing is that they have a position and a rotation.) These transforms are the points on which the other modules will attach on. 

![Attachment Points]()

Finally, I unwrap the mesh onto a empty UV with a simple pattern which will later be filled by the texture generator. 

![UV]()

Here are some other modules:
![Barrel]()
![Magazine]()

Once the models are finished I import them into Unity. 