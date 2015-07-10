---
published: false
---


![Furnace]()
Finally a big update!
<!--excerpt-->

##Environment
My main focus this week was to make the game look better. The most obvious changes are new Wall and floor tiles,
![New Tiles]()
(which also have some variations)
![Furnace]()
![Wall variatons]()

Additionally, some rooms now spawn flooded with water(which doesn't do anything yet):
![Water]()

Finally, there are now some random decorational elements like stones or graves:
![Stones]()
![Graves]()

##ASCII Effect
I tweaked the ASCII Effect to be less confusing by slightly increasing the resolution, as well as adjusting the backdrop darkness for the characters.
So instead of just taking the pixel brightness and dividing it by a fixed value, it is now divided by 1/8 to 8/8 (depending on which character is being displayed). With this in place colors ramp much more nicely.

![Altered ASCII Effect](http://i.imgur.com/TxvbQsh.png)
