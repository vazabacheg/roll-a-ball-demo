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
* Remove the template
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
* `FixedUpdate()` is called by the physics engine
  independent of the current frame rate.
  This ensures the best accuracy for the physics simulation.
* Review the documentation as you go and as demonstrated in the session.
* In the Unity editor, set the `speed` to `10`.

## Moving the Camera

* Select the Main Camera and move it up by 10 units (Position _y_ component)
  and tilt it down by 45 degrees (Rotation _x_ component).
* Make the camera a child of the Player game object
  by dragging it onto the Player in the scene hierarchy.
* Note that the camera not only follows the player but also rotates with it.
* Detach the Camera from the Player object
  by dragging it to the root of the scene hierarchy.
* Add a new script to the camera named `CameraController`.
* Move the new script into the `Scripts` folder.
* Remove the template
  and replace it with the following code:
```
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class CameraController : MonoBehaviour {

    	public GameObject player;

    	private Vector3 offset;

    	void Start () {
    		offset = transform.position - player.transform.position;
    	}

    	void LateUpdate () {
    		transform.position = player.transform.position + offset;
    	}
    }
```
* `LateUpdate()` is run after all other updates are finshed,
  making sure that all game objects are in their final positions.
  This ensures the camera being in a consistent position relative to the player.
* Fill the `Player` reference in the `CameraController` script
  by dragging the Player object from the scene hierarchy onto the corresponding slot
  in the script.

## Setting up the Play Area

* Create an empty game object to serve as a container for the four walls
  surrounding the play area.
  Make sure the transform is reset to the origin.
* Create a new cube called _West Wall_ and make it a child of the _Walls_ object.
* Scale the cube to `(0.5, 2, 20.5)`.
* Set the cube's position to `(-10, 0, 0)`.
* Duplicate the West Wall by selecting it and then using `Edit->Duplicate`.
* Rename it _East Wall_ and set the position to `(10, 0, 0)`.
* Duplicate the East Wall and rename it _North Wall_.
* Set its position to `(0, 0, 10)` and its scale to `20.5, 2, 0.5`.
* Duplicate the North Wall, rename it _South Wall_ and position it at `0, 0, -10`.

## Collecting, Scoring, and Building the Game

### Creating Collectable Objects

#### Create a Single Collectable Object that Rotates

* Create a cube, name it _Pick Up_ and set its transform to origin.
* Deactivate the Player object to remove it from the scene temporarily.
* Lift the pick up onto the plane by setting its _y_ position to `0.5`.
* Let the pick up float by scaling it to `(0.5, 0.5, 0.5)`
  and rotating it by `(45, 45, 45)`.
* Add a script to the pick up, call it `Rotator`,
  place it into the `Scripts` folder, and open it for editing.
* Replace the template with the following code:
```
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class Rotator : MonoBehaviour {

    	void Update () {
    		transform.Rotate(new Vector3 (15, 30, 45) * Time.deltaTime);
    	}
    }
```

#### Turn the Pick Up into a Prefab

* At the top of the project hierarchy, create a folder named `Prefabs`.
* Drag the pick up object from the scene hierarchy into the Prefabs folder.
  This automatically creates a prefab (a.k.a. template).
* In the scene hierarchy, create an empty game object named _Pick Ups_,
  reset its transform, and drag the pick up object into it.
* Select a top-down view of the scene by clicking on the _y_ gizmo.
* Zoom out to see the whole scene.
* Select the pick up and make sure the gizmo is in _Global_ mode.
* Move the pick up to the top of the game area.
* Duplicate the pick up 11 times and place them in a circle around the player.

####  Change the Color on the Prefab

* Duplicate the `Background` material and rename it `Pick Up`.
  Set the albedo RGB to `(255, 255, 0)` (yellow).
* Apply the material to the prefab by first selecting the prefab,
  then opening the `Materials` folder and dragging the `Pick Up` material onto the prefab.

### Collecting the Pick Up Objects

* Bring back the player object by enabling it again.
* Review the player object's sphere collider reference
  by clicking on the reference icon.
  Then, review the corresponding scripting interface by clickin on the
  _Switch to Scripting_ link.
* Review the `OnTriggerEnter()` reference.
* Add the following code to the `PlayerController` script
  directly under the end of the `FixedUpdate()` method:
```
    void OnTriggerEnter (Collider other) {
    		if (other.gameObject.CompareTag("Pick Up")) {
    			other.gameObject.SetActive(false);
    		}
    	}
  ```
* Add a tag named `Pick Up` to the pick up prefab.
* Select the `Pick Up` tag from the prefab's tag list to tag it.
* Test the game and watch nothing happen,
  because the player collider is dynamic
  but the pick up collider is static just like the walls,
  preventing the colliders from overlapping.
  * Change the prefab's collider into a trigger by checking `isTrigger`.
