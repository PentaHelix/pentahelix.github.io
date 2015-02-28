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

In the Indie Effects directory you will see a few sub-directories, the ones which are interesting to us are Classes/ and ShaderDir/Normal/. In these directories is everything you need to create your own shader. After looking at a few Effects, you will have realized that an Indie Effect is made up of 2 components: 1 JS Script + 1 Shader = 1 Effect. There's nothing stopping us from making our own now! Firstly, I created a new foder in my Assets for keeping everything clean. Now, we need a JS file and a shader fle. Both can be created from the context menu in the Project window. To start off, we simply create a shader that does nothing but acts as a template.

###JavaScript
```javascript
#pragma strict
@script RequireComponent(IndieEffects)
@script AddComponentMenu("Indie Effects/FX Skeleton")

import IndieEffects;
import UnityEngine.Texture;
var fxRes : IndieEffects;
private var mat : Material;
var pixelShader : Shader;

function Start () {
	fxRes = GetComponent(IndieEffects);
    mat = new Material(pixelShader);
}

function Update () {
	fxRes.RT.filterMode = FilterMode.Point;
    mat.SetTexture("_MainTex", fxRes.RT);
}
```
###Shader
```glsl
Shader "Custom/asciifyGL" {
	Properties {
		_MainTex ("Base (RGB)", 2D) = "white" {}
	}
	SubShader {
		Pass {
            ZTest Always Cull Off ZWrite Off Fog { Mode off }
           
       	 	CGPROGRAM
        	#pragma vertex vert_img
        	#pragma fragment frag
        	#include "UnityCG.cginc"
        	uniform sampler2D _MainTex;
        	float3 frag (v2f_img i):COLOR{
				return tex2D(_MainTex, i.uv);
        	}
        	ENDCG
        }
    }
}
```

Additionally, you need to add these components to your camera:
![Camera Components](http://i.imgur.com/CAzWNkf.png)
The Indie Effects Component can be added from the "Add Component" menu. Don't forget to set Texture Size to 512. Then, simply add your JS and the shader as the Pixel Shader property.

So, what happens in these file? The script initializes the Shader in Start(), and then sets the texture to the current RenderTexture in Updaste();
