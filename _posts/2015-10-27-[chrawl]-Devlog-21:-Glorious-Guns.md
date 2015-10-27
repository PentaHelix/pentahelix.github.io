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
```c#
public class GunGen{
	public static GameObject MakeGun(int r, int b, int m){
		<Instantiate Modules>
		<Create Empty GameObject ot server as parent>
        
		Transform gun = new GameObject("Gun").transform;

		rifle.parent = gun;
		barrel.parent = gun;
		magazine.parent = gun;

		rifle.name = "Rifle";
		barrel.name = "Barrel";
		magazine.name = "Magazine";

		rifle.localPosition = Vector3.zero;
		barrel.localPosition = Vector3.zero;
		magazine.localPosition = Vector3.zero;

		Align(barrel.Find("Rifle"),rifle.Find("Barrel"));
		Align(magazine.Find("Rifle"),rifle.Find("Magazine"));
		gun.transform.localScale = new Vector3(.4f,.4f,.4f);
		Gun gunC = gun.gameObject.AddComponent<Gun>();

		Transform muzzle = barrel.Find("Muzzle");
		muzzle.GetComponent<Renderer>().material = MonoBehaviour.Instantiate(Resources.Load("Materials/Meshes/MuzzleFlash")) as Material;
		muzzle.gameObject.AddComponent<Light>().color = new Color(1f,0.55f, 0.02f);
		muzzle.GetComponent<Light>().range = 4;
		muzzle.GetComponent<Light>().intensity = 3;

		gun.localPosition = new Vector3(.38f,-.46f,.6f);
		gun.localEulerAngles = new Vector3(10,10,0);
		gun.gameObject.AddComponent<Center>().center = gun.localPosition;

		Transform bulletPart = (MonoBehaviour.Instantiate(Resources.Load("Prefabs/Misc/BulletParticle")) as GameObject).transform;
		bulletPart.parent = gun;
		bulletPart.name = "BulletParticle";
		bulletPart.localPosition = new Vector3(.6f,.6f,.6f);
		bulletPart.localEulerAngles = new Vector3(30,90,0);

		gun.Find("Rifle").GetComponent<Renderer>().material = TexGen.MakeMaterial();
		gun.GetComponent<Gun>().modules = new int[]{r, b, m};

		gun.gameObject.AddComponent<AudioSource>().clip = Resources.Load("Audio/Shot") as AudioClip;
		gun.GetComponent<AudioSource>().playOnAwake = false;
		return gun.gameObject;
	}	
```
