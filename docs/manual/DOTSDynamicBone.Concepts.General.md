---
uid: DOTSDynamicBone.Concepts.General
title: General Concepts
---

#General Overview

DOTS Dynamic Bone is broken up into 5 Systems, 16 Jobs, and 3 main components.

The purpose of DOTS Dynamic Bone is to perform physics simulations on components within your model.
You can use this to perform simulations on cloths, hair, rope, etc. Using the benefits of Unity
DOTS, ECS, & Burst, systems were created to perform these calculations efficently with minimal performance
hits in your Game/Application.

##Components

DOTS Dynamic Bone contains 3 main component types (See the Components tab for more information:
- [Bones](xref:DOTSDynamicBone.Concepts.Bones)
	- [Bone](xref:DOTSDynamicBone.DOTSDynamicBone)
	- [Multiple Bones](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement)
	- [Independant Bone](xref:DOTSDynamicBone.IndependantDOTSDynamicBone)
- [Particles](xref:DOTSDynamicBone.Concepts.Particles)
- [Colliders](xref:DOTSDynamicBone.Concepts.Collisions):
	- [Preset Colliders](xref:DOTSDynamicBone.Concepts.Collisions.PresetCollisions)
	- [Natural Colliders](xref:DOTSDynamicBone.Concepts.Collisions.NaturalCollisions)

DOTS Dynamic Bone contains 5 [systems](xref:DOTSDynamicBone.Concepts.Systems):

*NOTE: this list is ordered by when the system's Update call is executed*

- DOTSDynamicBoneTransformOverrideSystem
- DOTSDynamicBoneCullingSystem
- DOTSDynamicBonePresetPhysicsCollisionSystem
- DOTSDynamicBoneNaturalCollisionSystem
- DOTSDynamicBoneUpdateSystem

DOTS Dynamic Bone performs 16 [jobs](xref:DOTSDynamicBone.Concepts.Jobs) that updates data based on the Components in the Entity. 
These Jobs include:

- System: DOTSDynamicBoneCullingSystem
	- DOTSDynamicBoneCullingUpdateTransformsJob
	- DOTSDynamicBoneMultiple	CullingUpdateTransformsJob
- System: DOTSDynamicBoneTransformOverrideSystem
	- DOTSDynamicBone_ParticleLocalToWorldOverrideJob
- System: DOTSDynamicBonePresetPhysicsCollisionSystem
	- DOTSDynamicBoneWithPresetCollidersJob
	- DOTSDynamicBoneMultipleWithPresetCollidersJob
	- IndependantDOTSDynamicBoneWithPresetCollidersJob
- System: DOTSDynamicBoneNaturalCollisionSystem
	- DOTSDynamicBoneNaturalCollisionUpdateJob
	- DOTSDynamicBoneMultipleNaturalCollisionUpdateJob
	- IndependantDOTSDynamicBoneNaturalCollisionUpdateJob
	- DOTSDynamicBoneIndependantMultipleNaturalCollisionUpdateJob
- System: DOTSDynamicBoneUpdateSystem
	- DOTSDynamicBoneUpdateJob
	- DOTSDynamicBoneMultipleUpdateJob
	- DOTSDynamicBoneCollisionFinishJob
	- DOTSDynamicBoneMultipleCollisionFinishJob
	- IndependantDOTSDynamicBoneJob
	- DOTSDynamicBoneIndependantMultipleJob
	- IndependantDOTSDynamicBoneJob
	- DOTSDynamicBoneIndependantCollisionFinishSystem


