---
uid: DOTSDynamicBone.Concepts.Systems
title: Systems
---

#Systems

DOTS Dynamic Bone contains 5 systems with each having a specific role and sepcfic execution location(Check the Systems tab for more information):

*NOTE: this list is ordered by when the system's Update call is executed*

- DOTSDynamicBoneTransformOverrideSystem
- DOTSDynamicBoneCullingSystem
- DOTSDynamicBonePresetPhysicsCollisionSystem
- DOTSDynamicBoneNaturalCollisionSystem
- DOTSDynamicBoneUpdateSystem


# DOTSDynamicBoneTransformOverrideSystem

The **DOTSDynamicBoneTransformOverrideSystem** is responsible for updating/overriding either the LocalToWorld, Translation, or Rotation ComponentData of an override's ```Entity```. This
system updates the ```Entity``` before the Build Physics World system which means that this system executes before collisions are performed globally. 

NOTE: a neat little consequence of this is that when overriding Transforms with DOTSDynamicBone and PhysicsJoints, by modifying the Translation and Rotation of the ```Entity``` you subsequently move the
PhysicsJoint which is turn moves the ```Unity.Physics.PhysicCollider```. This makes it so you can move ```Unity.Physics.PhysicColliders``` using **DOTS Dynamic Bone** and since **DOTS Dynamic Bone** works with
*Unity DOTS Animation* then your DOTS Animations can move the ```Unity.Physics.PhysicCollider``` as well. pretty neat.

# DOTSDynamicBoneCullingSystem

The **DOTSDynamicBoneCullingSystem** is responsible for performing culling checks. Each **DOTS Dynamic Bone** has culling data in it that makes it so if you exceed a distance from an Entity "Like a Player" then the *DOT Dynmaic Bone*
calculations are no longer done on the *Dynamic Bone Entity*. Culling is great for instances where you want to save some computation time by not performing caculations on entities ourside a Players view.

# DOTSDynamicBonePresetPhysicsCollisionSystem

The **DOTSDynamicBonePresetPhysicsCollisionSystem** is responsible for updating **DOTS Dynamic Bone** Entities with Preset collisions. 

# DOTSDYnamicBoneNaturalPhysicsCollisionSystem

The **DOTSDYnamicBoneNaturalPhysicsCollisionSystem** is responsible for updating **DOTS Dynamic Bone** Entities that use Natural DOTS Collision. 

# DOTSDynamicBoneUpdateSystem

The **DOTSDynamicBoneUpdateSystem** is responsible for a few things:
- Updating Non-Collision related **DOTS Dynamic Bone** Entites.
- Updating Entity information with Entities linked to an Independant **DOTS Dynamic Bone** Entity.
- Updating the ```AnimatedLocalToRoot``` data.