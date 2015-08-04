---
published: false
---


Hey everyone, Unity 5 was released recently (well, somewhat recently), and I thought I'd rewrite the tutorial for the ASCII shader to suit the Unity 5 Image Effects.

<!--excerpt-->

Hey everyone, Unity 5 was released recently (well, somewhat recently), and I thought I'd rewrite the tutorial for the ASCII shader to suit the Unity 5 Image Effects. I also made several improvements to it during development of chrawl. So here goes, ASCII Shader Mk.II

##ASCII Greyscale
To start off, you are going to need a texture with an ASCII representation of various greyscale values on it, ordered by brightness. I made mine in a sprite editor called [Aseprite](http://aseprite.org/), it should look somewhat like this:

![ASCII Ramp](http://imgur.com/a/sDF8S#oIYVhJj)

If you need some help coming up with characters, take a look at [this](http://paulbourke.net/dataformats/asciiart/), however keep in mind that if you draw your own characters, greyscale values may vary since your **B** might be bolder than my B, and therefore appear brighter.

Now with that out of the way, it's time to setup 

##The Scene

