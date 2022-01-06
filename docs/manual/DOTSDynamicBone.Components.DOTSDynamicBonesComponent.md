---
uid: DOTSDynamicBone.Components.DOTSDynamicBonesComponent
title: DOTSDynamicBones Component
---

#DOTSDynamicBones Component
The DOTSDynamicBonesComponent is the same as the DOTSDynamicBoneComponent except you can 
add multiple bons to a single Entity. This Component adds a DynmiacBuffer<DOTSDynamicBone>, DynamicBuffer<Particle>, and a 
DynamicBuffer<DOTSDynamicBoneCollider_BufferElement> (if colliders are added) to the entity during conversion. It is very
important that you add this to the root GameObject that has a RigComponent attached to it as well.

The parameters are the same so that's it!