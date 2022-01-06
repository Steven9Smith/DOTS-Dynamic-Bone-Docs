---
uid: DOTSDynamicBone.Concepts.Bones.SingleBone
title: Single Bone
---

# Single Bone

In **DOTS Dynamic Bone** a *Single Bone* is associated with entities that contain the [DOTSDynamicBone](xref:DOTSDynamicBone)
ComponentData. The [DOTSDynamicBone](xref:DOTSDynamicBone) data can be modified at any time. 

# General Single Bone Properties

Here I will go over some important properties and tips when working with the [DOTSDynamicBone](xref:DOTSDynamicBone) ComponentData.

### DOTSDynamicBone and DOTSDynamicBone_BufferElement do not share the same jobs.

since [DOTSDynamicBone](xref:DOTSDynamicBone) represents a single bone, a seperate ```Job``` is used for 
[DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement). Since 
[DOTSDynamicBone](xref:DOTSDynamicBone) holds less data than [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement)
and doesn't deal with inicies, it is generally faster to deal with. 

### UseNaturalColliders

this a ```bool``` associated with the [Natural Collision System](xref:DOTSDynamicBone.Concepts.Collisions.NaturalCollisions).

### UseAnimatedLocalToRoot

this is a ```bool``` that tells the ```DOTSDynamicBoneUpdateSystem``` whether or not to convert a [Particle's](xref:DOTSDynamicBone.Particle)
 transform data into ````Unity.Animation.AnimatedLocalToWorld``` transform data. If this is false then you will experience "jank" unless
you are centered in world coordinates. It is **highly recommended** you always set this to true. However, I leave the choice up to you.

### UpdateLocalToWorldTransforms, UpdateTranslationTransforms, UpdateRotationTransforms

These 3 ```bool``` values, when one of them is set to true, will add an transform override ComponentData to the *RigComponentEntity*.
See [DOTSDynamicBoneParticleLocalToWorldOverride_Tag](xref:DOTSDynamicBone.DOTSDynamicBoneParticleLocalToWorldOverride_Tag) for more information.

### m_RootEntity

This is the the Entity associated with the root particle. this value should match the value of the m_TransformEntity of the first 
[Particle](xref:DOTSDynamicBone.Particle) in the ```DynamicBuffer```. 

### m_ReferenceObjectEntity, m_ReferenceObject, and m_DistanceToObject

This data is used with the DOTSDynamicBoneCullingSystem. The m_ReferenceObjectEntity is the Entity used to the distance comparison.
The m_ReferenceObject is the m_ReferenceObjectEntity's transform data. and the m_DistanceToObject is the max distance the *RigComponentEntity*
and the m_ReferenceObjectEntity must be before the Bone is disabled. This data can be changed at anytime but in order for the
DOTSDynamicBoneCullingSystem to pick it up, you must have the [DOTSDynamicBoneCulling_Tag](xref:DOTSDynamicBone.DOTSDynamicBoneCulling_Tag)
 added to ```Entity``` as well.
 
Any other not mentioned should be looked at [here](xref:DOTSDynamicBone.DOTSDynamicBone)