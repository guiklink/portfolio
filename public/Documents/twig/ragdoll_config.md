## Configuring Colliders and Ragdoll Joints
The Ragdoll Wizard does a very good job for you in setting the raw ragdoll, however from my experience, if you want your character to be working properly and avoid having to redo work I strongly encourage you to spend some time doing a more detailed tuning on your character. 

### Colliders
The Unity physics engine uses the [colliders boxes](http://docs.unity3d.com/Manual/class-BoxCollider.html) to calculate collisions and you can use them in your scripts to trigger events. By default the Ragdoll Wizard usually makes the colliders way bigger than what you really need (see raw the colliders in green on the figure bellow), what makes the ragdoll to move in a clumsy and sometime even unexpected ways. For my character I made sure they were almost overlapping with  the real size of each respective rigid body. 

![Colliders](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/colliders.png?raw=true)

### Joints
Once again Unity does not do everything automatically for us. The ragdoll uses the [Character Joint](http://docs.unity3d.com/Manual/class-CharacterJoint.html) component that gives you three preconfigured axes of rotations. Make sure to put the orange gizmos as the axis of rotations for places that do not rotate back like knees and elbows, this is the only axis you can have different values for maximum and minimum rotation, while the others axis will rotate the same amount of degrees for both sides (if you configure with 40 it will rotate from -40 to +40). Finally, make sure each joint is rotating as much as you will need on each axis, usually the arms require the most attention. In the figure bellow I show an example of how the joints in the arm should be set.

![Joints](https://github.com/guiklink/portfolio/blob/gh-pages/public/images/twig/JointsConfig.png?raw=true)

### Foot Rigid Body
By default the Ragdoll wizard does not add a [rigid body](http://docs.unity3d.com/Manual/class-Rigidbody.html) for the foot. In my implementation of Twig the forces that move the leg are added in the foot, the upper leg controller's only function is to lock undesired rotations so the character does not move in undesired ways. Therefore, I added a rigid body for the each foot and attached it to its respective leg through a [Character Joint](http://docs.unity3d.com/Manual/class-CharacterJoint.html). 

