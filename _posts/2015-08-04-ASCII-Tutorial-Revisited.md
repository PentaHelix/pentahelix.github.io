---
published: false
---



Hey everyone, Unity 5 was released recently (well, somewhat recently), and I thought I'd rewrite the tutorial for the ASCII shader to suit the Unity 5 Image Effects.

<!--excerpt-->

Hey everyone, Unity 5 was released recently (well, somewhat recently), and I thought I'd rewrite the tutorial for the ASCII shader to suit the Unity 5 Image Effects. I also made several improvements to it during development of [chrawl](http://pentahelix.github.io). So here goes, ASCII Shader Mk.II

##ASCII Greyscale
To start off, you are going to need a texture with an ASCII representation of various greyscale values on it, ordered by brightness. I made mine in a sprite editor called [Aseprite](http://aseprite.org/), it should look somewhat like this:

![ASCII Ramp](http://i.imgur.com/oIYVhJj.png)

This is basically a texture which contains characters in ascending brightness in a grid of 31px * 52px. The grid can be any size, just remember to adjust the shader later on to fit your grid.

If you need some help coming up with characters, take a look at [this](http://paulbourke.net/dataformats/asciiart/), however keep in mind that if you draw your own characters, greyscale values may vary since your **B** might be bolder than my B, and therefore appear brighter.

Now with that out of the way, it's time to setup 

##The Scene
For this tutorial I created a basic scene with a few shapes and colors.
![Scene](http://i.imgur.com/kOJy7J4.png)

##The Script
In Unity 5, an ImageEffect is made up of 2 components: a Shader to do the computing on the GPU, as well as a Script to control the shader. Let's start with the Script. Here's a basic Unity PostEffectBase: 

```c#
using System;
using UnityEngine;
namespace UnityStandardAssets.ImageEffects{
	[ExecuteInEditMode]
	public class ASCIIScript:PostEffectsBase{
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
If you know c#, this should all make sense to you, if you are using UnityScript learn c# :P (alternatively just accept this as working).

Here is the script we'll be using:
```c#
using System;
using UnityEngine;
namespace UnityStandardAssets.ImageEffects{
	[ExecuteInEditMode]
	public class ASCIIScript:PostEffectsBase{
		public Shader ASCIIShader;
		private Material m_ASCII;
		public Texture2D CharTex;
		public float tilesX = 160;
		public float tilesY = 50;
		public float darkness = .8f;

		public override bool CheckResources (){
            CheckSupport(false);
            m_ASCII = CheckShaderAndCreateMaterial(ASCIIShader, m_ASCII);
            
            if (isSupported){
				m_ASCII.SetTexture("_CharTex", CharTex);
				
				m_ASCII.SetFloat("_tilesX", tilesX);
				m_ASCII.SetFloat("_tilesY", tilesY);

				m_ASCII.SetFloat("_tileW", 1/tilesX);
				m_ASCII.SetFloat("_tileH", 1/tilesY);

				m_ASCII.SetFloat("_darkness", darkness);
            }
            return isSupported;
        }

		private void OnRenderImage(RenderTexture source, RenderTexture destination){
			Graphics.Blit(source, destination, m_ASCII);
		}
	}
}
```

You can see that the screen is 160 by 50 characters wide/high, and the brightness is .8f. in CheckResources (called when the shader is initialized) we create the material, check for support, and set the shaders variables. OnRenderImage is called once per tick and runs the current frame through the shader. You need to set the ASCIIShader variable to the shader we'll create in the Inspector. With the script done, let's tackle the shader.

##The Shader
Here's a basic one:

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

As you can see, those are all already fed to the shader via the script.

For the ASCII shader we start by pixelating the screen:
```glsl
half2 uv = half2((int)(i.uv.x / _tileW) * _tileW + _tileW / 2, (int)(i.uv.y / _tileH) * _tileH + _tileH / 2);
float4 c = tex2D(_MainTex, uv);
```
(this is in the frag function, however if you don't have basic shader knowledge you should probably read about that first)
Now that we have the color we want, we need to get its brightness, I'm using the [simplified Liminance formula](http://stackoverflow.com/questions/596216/formula-to-determine-brightness-of-rgb-color).
```glsl
int b = (int)((c.r*2+c.g*5+c.b*1)/_darkness);
``` 
Next:
```glsl
float onSpriteX = (i.uv.x % _tileW) * _tilesX;
float onSpriteY = (i.uv.y % _tileH) * _tilesY;
```
This calculates the coordinates of the current pixel on the charactermap made earlier. By modulusing (is that even a word?) [i.uv.x] with [\_tileW], onSpriteX cycles from 0 to [\_tileW] [\_tilesX] times. Since [\_tileW] = 1 / [\_tilesX], [\_tileW] * [\_tilesX] = 1. This multiplication means that the cycle goes from 0 to 1 [\_tilesX] times, i.e. once per character. The same principle is used for onSpriteY. 

```glsl
half2 charCoords = half2((b * 31.0f / 300.0f)+(onSpriteX*31.0f/300.0f), (onSpriteY));
float3 charMask = tex2D(_CharTex, charCoords);
```

This line gets the actual pixel we want on the character map. Some information: The character map is 300px wide, and contains 8 characters, each 31px wide. So in glsl terms, 31/300 is one character. Take the brightness [\b] and multiply is by one character, to get to the character corresponding to the brightness. Then, we use the variables onSpriteX (cycling 0 to 1 for each character's x/y) to read the pixel we are currently at. If we were to return charMask, we would get every tile mapped to a character, but in black and white, just like our character map:
![Black and White ASCII](http://i.imgur.com/QOq20WU.png)

In order to get color we need to use the character as a mask for our color.

```glsl
if(charMask.r == 1.0){
	return c;
}else{
	c *= (b+2)/10.0f;
	return c;
}
```
This basically checks if the current pixel on the charmap is white (the pixel is on the character) or not (on the background). If yes this simply returns the color of main texture, if not it returns the color, but darkend.

And that's it!

![ASCII Effect](http://i.imgur.com/8RVlEg1.png)

I hope you were  able to follow the tutorial, it'll probably take some time to wrap your head around this. If you would like to see any other novelty shader covered, or have any suggestions, feedback, noticed a mistake, leave comment and let me know!