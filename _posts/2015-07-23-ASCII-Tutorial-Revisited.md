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
For this tutorial I created a basic scene with a few shapes and colors.
![Scene](http://imgur.com/a/sDF8S#kOJy7J4)

##The Shader
In Unity 5, an ImageEffect is made up of 2 components: a Shader to do the computing on the GPU, as well as a Script to control the shader. Let's start with the Shader. Here's a basic Unity Shader: 
```glsl
Shader "Custom/ASCIIShader" {
    Properties {
        _MainTex ("Base", 2D) = "white" {}
    }
    SubShader {
        Pass{
            CGPROGRAM
            #pragma fragment frag
            #pragma vertex vert_img
            #pragma target 3.0
            #include "UnityCG.cginc"
    
            struct v2f {
                float4 pos : SV_POSITION;
                float2 uv  : TEXCOORD0;
            };
            sampler2D _MainTex;
            float4 frag(v2f i) : COLOR{
            	return tex2D(_MainTex, i.uv);
            }
            ENDCG
        }
    }
    FallBack off
}
```

This is a blank shader that simply returns the texture it is given in our case the current frame. For our shader we will need a few values:

|Value   |Description                                    |
|--------|---------------------------------------------- |
|tilesX  |How many characters are displayed horizontally?|
|tilesY  |How many characters are displayed vertically?  |
|tileW   |How wide is one character?                     |
|tileH   |How high is one character?                     |
|darkness|Overall darkness of the rendered image?        | 

More on those later, when we feed them to the shader via our script, so don't worry if you see those in the shader code, they weill be addressed a bit later

For the ASCII shader we start by pixelating the screen:
```glsl
half2 uv = half2((int)(i.uv.x / _tileW) * _tileW + _tileW / 2, (int)(i.uv.y / _tileH) * _tileH + _tileH / 2);
float4 c = tex2D(_MainTex, uv);
```
(this is in the frag function, however if you don't have basic shader knowledge you should probably read about that first)
Now that we have the color we want, we need to get it's brightness, I'm using the [simplified Liminance formula](http://stackoverflow.com/questions/596216/formula-to-determine-brightness-of-rgb-color).
```glsl
	int b = (int)((c.r*2+c.g*5+c.b*1)/_darkness);
``` 

```c#
using System;
using UnityEngine;
namespace UnityStandardAssets.ImageEffects{
	[ExecuteInEditMode]
	public class ASCIIScript:PostEffectsBase{
		//Variables required for the ImageEffect
		public Shader shader;
		private Material mat;

		public override bool CheckResources (){
            CheckSupport(false);
            mat = CheckShaderAndCreateMaterial(shader, mat);
            return isSupported;
        }

		private void OnRenderImage(RenderTexture source, RenderTexture destination){
			Graphics.Blit(source, destination, mat);
		}
	}
}
```
If you know c#, this should all make sense to you, if you are using UnityScript learn c# :P (alternatively just accept this).

