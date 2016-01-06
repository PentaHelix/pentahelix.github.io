---
published: true
---



##Preparing a Plane
A simple plane will be the base of our terrain.
![Add Plane](http://i.imgur.com/r1j0arW.png)
(I scaled the plane up by 10)

Once you have your plane, in order to create low-poly terrain, we need more polygons. In edit mode, hit [W] to open up the "Special Actions" menu and select "Subdivide". You should see a small window pop up on the left. In that window you can select how many subdivisions you want. For this tutorial I am using 20.
![Subdivided](http://i.imgur.com/tYcPl1G.png)
However, for most low-poly art, triangles work a lot better than squares, so in order to make our quads to triangles, add a "Triangulate" modifier from the object Modifier tab.
![Triangles](http://i.imgur.com/HdXC86L.png)
(Using "Fixed" for quad mode creates more unified triangles)

After applying the modifier, things are still looking pretty flat. To create some noise in the terrain, add a "Displace" modifer. Set midlevel to 0 and press the button to assign it a new texture.
![Displacement](http://i.imgur.com/GAzd0xl.png)

The right-most button next to the Texture should take you to the texture tab.
To add some distortion to the plane, change the Texture type to "Clouds", change to color mode to "Color", and set the RGB Multiply values for G and B to 0. Tweak the R value, as well as "Brightness", "Contrast", and "Saturation" until you find a suiting level of Displacement. (If you are in object mode, you can see the changes in realtime).
![Texture Tab](http://i.imgur.com/YRd2bfa.png)


TO BE CONTINUED