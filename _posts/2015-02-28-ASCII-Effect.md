---
published: false
---

## 3D ASCII Effect

I recently posted about my WIP game chrawl on the roguelikedev subreddit. A few people showed interest in how I achieved my 3d ASCII effect, so this post will explain how you can ASCII in 3d too!

![3d ASCII Demo](http://i.imgur.com/Mm5OksT.gif)
Here is the 3d effect in all its glory.

Now, let's get to the technical aspect of things. In order to schieve this, all you should need is a 3d engine/framework which supports post processing. I used unity for the quick setup because I wanted to get this running without writing a 3d framework first. If you want to use Unity as well, you might be thinking that Post Processing requieres Unity Pro. It does, but with a package called [Indie Effects](http://forum.unity3d.com/threads/indieeffects-bringing-almost-aaa-quality-post-process-fx-to-unity-indie.198568/) you can achieve similar effects to the ones in Unity Pro. I'll leave it to you to install Indie Effects and setup a simple scene you want to ASCIIfy, but after setting everything up, your project should look something like this:

![Basic Unity Project](http://i.imgur.com/6zsevcC.png)
Notice that the IndieEffects Package is already installed

In the Indie Effects directory you will see a few sub-directories, the ones which are interesting to us are Classes/ and ShaderDir/Normal/. In these directories is everything you need to create your own shader.