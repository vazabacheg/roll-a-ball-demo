# Steps of the 3D Roll a Ball Unity Demo Game

The tutorial can b found at
<https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial>.

## Environment and the Player

### Set up the Game

* Create a new 3D Project named "Roll a Ball"
* Save the scene into a newly created `_Scenes` folder
  in the `Assets` directory and call it `MiniGame`.

### Create the Plane

* Create a new 3D game object of type _Plane_ and rename it to _Ground_.
  Make sure to reset the transform.
* Use the `F` key or use `Edit->Frame Selected` from the menu
  to view everything in the current scene.
* Disable the grid by deselecting `Gizmos->Show Grid`.
* Enable the grid on the selected object by selecting `Gizmos->Selection Wire`.
* Change the scale of the plane to `(2, 1, 2)`.

### Create the Player

* Create a sphere named `Player`,
  reset the transform, and use `F` to focus on the selected sphere.
* Lift the sphere onto the plane by setting its _y_ position to `0.5`.

### Add Color to the Plane

Adding color will increase the contrast between the sphere and the plane.
Color is achieved via materials.

* Create a new folder named `Materials`.
* In the new folder, create a new material named `Background`.
* Set the RGB value for the albedo to `(0, 32, 64)`.
* Select the material in the project view and drag it onto the plane.


### Change the Light Direction

Changing the light will enable us to later on better view the player.

* Select `Directional Light` in the scene hierarchy.
* Set the _y_ rotation to `60`.

## Moving the Player

### Add a Rigidbody Component to the Sphere

This will allow the sphere to use physics to roll around
and collide with other objects in the game.

* Add a rigidbody component by selecting the sphere
  and select `Component->Physics->Rigidbody`.
* Create a new folder named `Scripts` in the `Assets` folder.
* Add and attach a script to the Player object by selecting it in the scene view
  and then, from the inspector, use `Add Component->New Script`.
  Name the script `PlayerController` and make sure the language is C# (C Sharp).
  When done, click `Create and Add`.
* Move the script into the `Scripts` folder.
* Open the script for editing by double-clicking on it.
* Remove both the `start()`` and `update()` methods
  and replace it with the following code:
```
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class PlayerController : MonoBehaviour {

    	public float speed;

    	private Rigidbody rb;

    	void Start ()
    	{
    		rb = GetComponent<Rigidbody> ();
    	}

    	void FixedUpdate ()
    	{
    		float moveHorizontal = Input.GetAxis ("Horizontal");
    		float moveVertical = Input.GetAxis ("Vertical");

    		Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);

    		rb.AddForce (movement * speed);
    	}
    }
```
* Review the documentation as you go and as demonstrated in the session.
* In the Unity editor, set the `speed` to `10`.
