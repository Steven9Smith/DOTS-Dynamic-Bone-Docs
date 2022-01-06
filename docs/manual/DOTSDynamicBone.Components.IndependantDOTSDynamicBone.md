---
uid: DOTSDynamicBone.Components.DOTSDynamicBoneComponent
title: DOTSDynamicBone Component
---

#DOTSDynamicBone Component

This Component adds a DOTSDynamicBone, DynamicBuffer<Particle>, and a 
DynamicBuffer<DOTSDynamicBoneCollider_BufferElement> (if colliders are added) to the entity during conversion. It is very
important that you add this to the root GameObject that has a RigComponent attached to it as well.

// put here or something

#Parameters

The parameters for this are accessable from [DOTSDynamicBoneClass](xref:DOTSDynamicBone.DOTSDynamicBoneClass).
These parameters are used for the conversion and initialization process of DOTS Dynamic Bones.

#RigComponent

Attach the RigComponent from the desired GameObject in here. *remeber: The RigComponent should be in the location as DOTSDynamicBoneComponnet...maybe I should autodetect it?*

// insert image of RigComponent option here

#Name

Ignore this, this is for the Editor during PlayMode. Rhis is not stored or used in any other way.

//insert image of Name

#Disable

Set this to true to disable to the bone during conversion. Due to the nature of how AnimatedLocalToRoot
works, disbabling a bone or particle doesn't stop all the calculations. the Particles still need to be
updated relative to thier parent so the only calculations that are disabled are the physics calculations.

**Note you can call Disable() on either a DOTSDynamicBone or a Particle anytime during runtime to disable it.**

**Another Note: be sure to set the bone and/or particle after you make changes to it (remeber, no references allowed)**

#Update Local To World Transforms

Set this to true in order update the associated particle's entity with the ParticleTransform data.

**NOTE: this will execute adter ExportPhysicsWorld and will overwrite any collisions done to the Entity during that time step. This is subject to change**

#Use Animated Local To Root

Set this to true to convert the LocalToWorld calculations into AnimatedLocalToRoot values. This should be also
set to true. 

#Rig Component

This is the RigComponent that comes with the Unity.Animation package. Simply add it to the Root GameObject,
then drag it into the RigComponent field. **This is Required**

#Root

Drag the Root GameObject in this field. **This is required**

#Update Rate

This is the internal simulation rate.

#Update Mode
Internal physics simulation type.

#Damping Distribution
How much the bones slowed down over a distribution curve.

#Elasticity
How much the force applied to return each bone to original orientation.

#Elasticity Distribution
How much the force applied to return each bone to original orientation over a distribution curve.

#Stiffness
How much bone's original orientation are preserved.

#Stiffness Distribution

How much bone's original orientation are preserved over a distribution curve.

#Inert
How much character's position change is ignored in physics simulation.

#Inert Distribution

How much character's position change is ignored in physics simulation over a distribution curve.

#Friction
How much the bones slowed down when collide.

#Friction Distribution
How much the bones slowed down when collide over a distribution curve.

#Radius
Each bone can be a sphere to collide with colliders. Radius describe sphere's size.

#Radius Distribution
Each bone can be a sphere to collide with colliders. Radius describe sphere's size over a distribution curve.

#End Length
If End Length is not zero, an extra bone is generated at the end of transform hierarchy.</value>
		
#End Offset
If End Offset is not zero, an extra bone is generated at the end of transform hierarchy.

#Gravity
The force apply to bones. Partial force apply to character's initial pose is cancelled out.

#Force
The force apply to bones.

#Colliders
Collider objects interact with the bones.

#Exclusions
Bones excluded from physics simulation.

#Freeze Axis
Constrain bones to move on specified plane.

#Distant Disable
Disable physics simulation automatically if Entity is far from the m_ReferencedObject Entity.

#Reference Object
Entity to be referenced for m_DistantDisable

#Distance To Object
Distnace the Entity has to be from the referenced Entity