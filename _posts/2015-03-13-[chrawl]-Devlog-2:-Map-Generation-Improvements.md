---
published: true
layout: post
---

![Torches](http://i.imgur.com/ziweZ21.gif)
This week: Torches and gelatinous cubes added to map generation, as well as visual improvements.

<!--excerpt-->

##Map Improvements
First up this week, here is the progress made on spawning enemies on the map, as well as spawning torches on certain walls.
![Spawning Cubes](http://i.imgur.com/GGBFf72.gif)

![Spawning Torches](http://i.imgur.com/ziweZ21.gif)
Torches really make the map look better

##Visual Improvements
As you may have noticed on the torch gifs, the camera is now bobbing up and down, and hands follow the camera a bit more smoothly.
![Bobbing](http://i.imgur.com/C3zdJFo.gif)

![Smooth Hands](http://i.imgur.com/BfCjGkv.gif)

I also got some work done on randomly generating spells for tomes/scrolls. 

Basically, I'm implenenting a flag system which consist of many different flags that can be any value between 0 and 1. These flags completely define a spell, and can be saved to act as a seed for spells. Flags are split up into effect flags and visual flags, but some flags influence others, for example the flag for fire damage influences the flag for the color red.

##Bonus
Too much bobbing? There's no such thing as too much bobbing.
![More Bobbing](http://i.imgur.com/yhueOor.gif)