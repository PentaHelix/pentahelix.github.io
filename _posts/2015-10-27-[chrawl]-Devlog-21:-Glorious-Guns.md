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
		[Instantiate Modules]
		[Create Empty GameObject to serve as parent]
        [Align attachment points using Align()]
		[Setup various Components]
        [Add procgen Texture]
        [Return gameobject]
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
Currently, texture generation is very simple, but it works pretty well. Basically, I choose 2 colors, map them to a pattern, choose a shade of grey, map it to a pattern, and then map those patterns to a UV. Sounds pretty simple right?

```c#
public class TexGen{
	public static List<Color> colors1 = new List<Color>{
   		new Color32(23, 255, 0, 255),
		new Color32(214, 232, 12, 255), 
		new Color32(255, 200, 0, 255),
		new Color32(232, 127, 12, 255),
		new Color32(255, 45, 0, 255)};

	public static List<Color> colors2 = new List<Color>{
    	new Color32(255, 0, 168, 255),
		new Color32(130, 12, 232, 255), 
		new Color32(0, 28, 255, 255), 
		new Color32(12, 174, 232, 255),
		new Color32(0, 255, 156, 255)};

	public static Material MakeMaterial(){
		Material m = MonoBehaviour.Instantiate(Resources.Load("Materials/Basic/ProcTex")) as Material;
		m.SetTexture("_Pattern1", MonoBehaviour.Instantiate(Resources.Load("Sprites/ProcTex/Patterns1/pat0"+((int)(Random.value * 5)+1))) as Texture2D);
		m.SetTexture("_Pattern2", MonoBehaviour.Instantiate(Resources.Load("Sprites/ProcTex/Patterns2/pat0"+((int)(Random.value * 2)+1))) as Texture2D);
		m.SetColor("_Color1", colors1.Random());
		m.SetColor("_Color2", colors2.Random());
		m.SetColor("_Color3", Color.white * (Random.value * 0.3f + 0.2f));
		m.SetVector("_PatMod1", new Vector3(Random.value * .5f, Random.value * .5f, Mathf.Max(1f, Random.value * 4f)));
		m.SetVector("_PatMod2", new Vector3(Random.value * .5f, Random.value * .5f, Mathf.Max(4, Random.value * 10f)));
		return m;
	}
}
```

In my Sprites/ProcTex/Patterns* folders are patterns that are randomly selected and sent to the shader. Patterns in Patterns1 are more random, and Patterns in Pattners2 are more geometric/regular, to give the guns a more consistent feel.

![Pattern 1](http://imgur.com/WkgGynK.png)

![Pattern 2](http://imgur.com/nnnigIs.png)

![Pattern 3](http://imgur.com/NRoZ0uf.png)

![Pattern 4](http://imgur.com/5QMQvQf.png)

_Color1 and _Color2 are chosen from 2 lists which are designed to result in high contrast colors. (List.Random() is an extension method, it simply returns a random element from the list).

_Color3 is a shade of grey in a certain range.

_PatMod1 and _PatMod2 are x/y translation and a scale factor to give patterns more variety.

The shader then simply creates the patterns and maps them to different sections of the UV.

```glsl
Shader "Custom/ProcTex" {
	Properties {
		_Color1 ("Color 1", Color) = (1,1,1,1)
		_Color2 ("Color 2", Color) = (1,1,1,1)
		_Color3 ("Color 3", Color) = (1,1,1,1)
		_Pattern1 ("Albedo (RGB)", 2D) = "white" {}
		_Pattern2 ("Albedo (RGB)", 2D) = "white" {}
		_PatMod1 ("Pattern 1 Modifiers", Vector) = (1,1,1)
		_PatMod2 ("Pattern 1 Modifiers", Vector) = (1,1,1)
	}


	SubShader {
		Tags { "RenderType"="Opaque"}
		Pass {
			Name "BASE"
			
			CGPROGRAM
			#pragma vertex vert_img
			#pragma fragment frag

			#pragma multi_compile_fog

			#include "UnityCG.cginc"

			sampler2D _Pattern1;
			sampler2D _Pattern2;

			fixed4 _Color1;
			fixed4 _Color2;
			fixed4 _Color3;

			float3 _PatMod1;
			float3 _PatMod2;


			fixed4 frag (v2f_img i) : SV_Target{
				fixed4 c1 = _Color1;
				fixed4 c2 = _Color2;
				fixed4 c3 = _Color3;
				if(i.uv.x <= .5f && i.uv.y <= .5f){
					half2 uvPat = i.uv/_PatMod1[2]*2;
					uvPat.x += _PatMod1[0];
					uvPat.y += _PatMod1[1];
					if(tex2D(_Pattern1, uvPat).a == 1){
						return c1.rgba;
					}else{
						return c2.rgba;
					}
				}
				
				if(i.uv.y >= 0.5){
					half2 uvPat = half2(i.uv.x/_PatMod2[2], i.uv.x/_PatMod2[2]*2);
					uvPat.x += _PatMod2[0];
					uvPat.y += _PatMod2[1];
					if(tex2D(_Pattern2, uvPat).a == 1){
						return c3.rgba;
					}else{
						return c3 * 0.5f;
					}	
				}

				return c3*.5f;
			}
			ENDCG			
		}
	} 

	Fallback "VertexLit"
}

```

##Where to go from here
The thing I am working on right now is module properties, which is going to be a json that lists certain properties for every module, such as tags, custom textures, ... . This makes it possible to have a module requiere certain tags from other modules to create more fine-tuned module synergies. 

I also want to expand the modularity of this system, e.g. separate the gun body into more modules. The number of module permutations grows exponentially to the number of modules per gun, so this would increase the gun variety drastically.

Any other improvements/suggestions? Leave a comment and let me know!

See you in a fortnight
~Jakob
