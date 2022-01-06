---
uid: DOTSDynamicBone.Concepts.Collision.Tips
title: Tups
---

# Tips

Due to how Unity's Physic System works I wanted to give you some tips on how to get the desired result with 
**DOTS Dynamic Bone**. 

## How do Use collisions without the collisions affecting my Entity?

Add the PhysicsExclude Component to your GameObject or Entity. This allows your entity to have Unity's Physics-realted
data without having it partake in Unity's Physics simulation. 

## I checked the box "Use Natural Colliders" but collisions are not working?

Perform these checks when trying to setup collision:

- did you add a physics collider to the desired Gameobject/Entity?
- are the collision filters preventing the physic colliders from contacting each other?
- did you check the Override Trasnform options?

## Can I get the collision to move with my animation?

Yes, if you use the override options it will move the Entity associated with it which will in turn,
move the physics collider. NOTE: static and kinematic bodies may not act as expected. Please make sure you
understand how Unity's physics system works on these bodies before using them.

## My Entity keeps getting moved by other colliders when it touches my DOTS Dynamic Bone collider!

This is normal behavior. if you only want the **DOTS Dynamic Bone** affected entities to move then I suggest adding
a ```PhysicExclude``` component to it. If this is desired but the effect is too strong, i recommend playing around with the entity's
mass. 
