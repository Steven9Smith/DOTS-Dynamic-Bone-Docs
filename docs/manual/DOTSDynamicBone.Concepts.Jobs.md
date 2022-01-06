---
uid: DOTSDynamicBone.Concepts.Jobs
title: Jobs
---

#Jobs

DOTS Dynamic Bone performs 18 jobs that updates data based on the Components in the Entity.
These Jobs include:

- System: DOTSDynamicBoneCullingSystem
	- DOTSDynamicBoneCullingUpdateTransformsJob
	- DOTSDynamicBoneMultipleCullingUpdateTransformsJob
- System: DOTSDynamicBoneTransformOverrideSystem
	- DOTSDynamicBone_ParticleLocalToWorldOverrideJob
- System: DOTSDynamicBonePresetPhysicsCollisionSystem
	- DOTSDynamicBoneWithPresetCollidersJob
	- DOTSDynamicBoneMultipleWithPresetCollidersJob
	- IndependantDOTSDynamicBoneWithPresetCollidersJob
	- IndependantDOTSDynamicBoneMultipleWithColliders
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
	- DOTSDynamicBoneIndependantCollisionFinishSystem

Now i will go over each job and give a breif description:

# DOTSDynamicBoneCullingSystem

Both *DOTSDynamicBoneCullingUpdateTransformsJob* and *DOTSDynamicBoneMultipleCullingUpdateTransformsJob* is
responsible for Updating data related to the culling system. The difference between them is the that one
modifies Entities with the [DOTSDynamicBone](xref:DOTSDynamicBone.DOTSDynamicBone) data component and the
other modifies Entities with the [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement)
 ```DynamicBuffer``` d.
```
	Entities
		.WithName("DOTSDynamicBoneCullingUpdateTransformsJob")
		.WithBurst()
		.WithAll<DOTSDynamicBoneCulling_Tag>()
		.WithReadOnly(GetLTW)
		.ForEach((ref DOTSDynamicBone bone) => {
			...
		})
		.ScheduleParallel();
```
```
	Entities
		.WithName("DOTSDynamicBoneMultipleCullingUpdateTransformsJob")
		.WithBurst()
		.WithAll<DOTSDynamicBoneCulling_Tag>()
		.WithReadOnly(GetLTW)
		.ForEach((ref DynamicBuffer<DOTSDynamicBone_BufferElement> bones) => {
			...
		})
		.ScheduleParallel();
```

# DOTSDynamicBoneTransformOverrideSystem

The *DOTSDynamicBone_ParticleLocalToWorldOverrideJob* is responsible for updating entities that are associated
with **DOTS Dynamic Bone** Entities that want the ```LocalToWorld```, ```Translation```, or ```Rotation``` 
values overwritten with the **DOTS Dynamic Bone** values.

```
	var OverrideJobHandle = Entities
		.WithName("DOTSDynamicBone_ParticleLocalToWorldOverrideJob")
		.WithBurst()
			.ForEach((in DynamicBuffer<Particle> particles, in DOTSDynamicBoneParticleLocalToWorldOverride_Tag tag) =>
			{
				...
			})
			.Schedule(Dependency);
	OverrideJobHandle.Complete();
```

# DOTSDynamicBonePresetPhysicsCollisionSystem

 The *DOTSDynamicBoneWithPresetCollidersJob*, *DOTSDynamicBoneMultipleWithPresetCollidersJob*,
 *IndependantDOTSDynamicBoneWithPresetCollidersJob* are responsible for updating **DOTS Dynamic Bone**
 Entities associated with the Preset Collision System. The difference between them is the that one
modifies Entities with the [DOTSDynamicBone](xref:DOTSDynamicBone.DOTSDynamicBone) data component, the
other modifies Entities with the [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement)
and the other one modifies **DOTS Dynamic Bone** Entities with Independant attributes.

```

	Entities
			.WithName("DOTSDynamicBoneMultipleWithPresetCollidersJob")
			.WithBurst()
			.WithReadOnly(GetLTWR)
			.WithNone<IndependantDOTSDynamicBone_Tag>()
			.WithReadOnly(collisionWorld)
			.ForEach((Entity entity, ref DynamicBuffer<DOTSDynamicBone_BufferElement> bones,
				ref DynamicBuffer<Particle> particles,
				ref DynamicBuffer<DOTSDynamicBoneCollider_BufferElement> m_Colliders,
				in DynamicBuffer<Unity.Animation.AnimatedLocalToWorld> altw) =>
			{
				...
			})
			.ScheduleParallel();
```
```
	Entities
		.WithName("DOTSDynamicBoneWithPresetCollidersJob")
		.WithBurst()
		.WithNone<IndependantDOTSDynamicBone_Tag>()
		.WithReadOnly(GetLTWR)
			.WithReadOnly(collisionWorld)
		.ForEach((Entity e, ref DOTSDynamicBone bone, ref DynamicBuffer<Particle> particles,
		   ref DynamicBuffer<DOTSDynamicBoneCollider_BufferElement> m_Colliders,
		   in DynamicBuffer<Unity.Animation.AnimatedLocalToWorld> altw) =>
		{
			...
		})
	   .ScheduleParallel();
```
```
	Entities
		.WithName("IndependantDOTSDynamicBoneWithPresetCollidersJob")
		.WithBurst()
		.WithReadOnly(GetLTWR)
		.WithReadOnly(GetAnimatedLocalToWorld)
			.WithReadOnly(collisionWorld)
		.ForEach((ref DOTSDynamicBone bone, ref DynamicBuffer<Particle> particles,
		 ref DynamicBuffer<DOTSDynamicBoneCollider_BufferElement> m_Colliders, in IndependantDOTSDynamicBone_Tag tag) =>
		{
			...
		})
		.ScheduleParallel();
```
```
	Entities
		.WithName("IndependantDOTSDynamicBoneMultipleWithColliders")
		.WithBurst()
		.WithReadOnly(GetLTWR)
			.WithReadOnly(collisionWorld)
		.WithReadOnly(GetAnimatedLocalToWorld)
		.ForEach((ref DynamicBuffer<DOTSDynamicBone_BufferElement> bones, ref DynamicBuffer<Particle> particles,
		 ref DynamicBuffer<DOTSDynamicBoneCollider_BufferElement> m_Colliders, in IndependantDOTSDynamicBone_Tag tag) =>
		{
			...
		})
		.ScheduleParallel();
```
 
 NOTE: the ```AnimtedLocalToRoot``` data isn't updated in these jobs.

# DOTSDynamicBoneNaturalPhysicsCollisionSystem

 The *DOTSDynamicBoneNaturalCollisionUpdateJob* *DOTSDynamicBoneMultipleNaturalCollisionUpdateJob*
	*IndependantDOTSDynamicBoneNaturalCollisionUpdateJob* *DOTSDynamicBoneIndependantMultipleNaturalCollisionUpdateJob* 
	are responsible for updating **DOTS Dynamic Bone**
 Entities associated with the Natural Collision System. The difference between them is the that one
modifies Entities with the [DOTSDynamicBone](xref:DOTSDynamicBone.DOTSDynamicBone) data component, the
other modifies Entities with the [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement)
and the other one modifies **DOTS Dynamic Bone** Entities with Independant attributes.

```

	Entities
		.WithName("DOTSDynamicBoneNaturalCollisionUpdateJob")
		.WithBurst()
		.WithNone<DOTSDynamicBone_BufferElement>()
		.WithNone<DOTSDynamicBoneCollider_BufferElement>()
		.WithNone<IndependantDOTSDynamicBone_Tag>()
		//.WithReadOnly(GetLTWR)
		.WithReadOnly(collisionWorld)
		.ForEach((Entity e, ref DOTSDynamicBone bone, ref DynamicBuffer<Particle> particles,
			  in DynamicBuffer<Unity.Animation.AnimatedLocalToRoot> altr,
		   in LocalToWorld coreLTW,
		   in DynamicBuffer<Unity.Animation.AnimatedLocalToWorld> altw,
		   in DOTSDynamicBoneNaturalCollider_Tag tag) =>
		{
			...
		})
	.ScheduleParallel();
```
```
	Entities
		.WithName("DOTSDynamicBoneMultipleNaturalCollisionUpdateJob")
		.WithBurst()
		.WithNone<DOTSDynamicBoneCollider_BufferElement>()
		.WithNone<IndependantDOTSDynamicBone_Tag>()
		.WithReadOnly(collisionWorld)
		.ForEach((Entity e, DynamicBuffer<DOTSDynamicBone_BufferElement> bones, ref DynamicBuffer<Particle> particles,
			  in DynamicBuffer<Unity.Animation.AnimatedLocalToRoot> altr, in LocalToWorld coreLTW,
		   in DynamicBuffer<Unity.Animation.AnimatedLocalToWorld> altw, in DOTSDynamicBoneNaturalCollider_Tag tag) =>
		{
			...
		})
	.ScheduleParallel();
```
```
	Entities
			.WithName("IndependantDOTSDynamicBoneNaturalCollisionUpdateSystem")
			.WithNone<DOTSDynamicBoneCollider_BufferElement>()
			.WithNone<DOTSDynamicBone_BufferElement>()
			.WithBurst()
			.WithReadOnly(GetLTWR)
			.WithReadOnly(GetAnimatedLocalToWorld)
			.WithReadOnly(collisionWorld)
			.ForEach((ref DOTSDynamicBone bone, ref DynamicBuffer<Particle> particles, in IndependantDOTSDynamicBone_Tag tag, in DOTSDynamicBoneNaturalCollider_Tag tag2) =>
			{
				...
			})
			.ScheduleParallel();
```
```
	Entities
		.WithName("DOTSDynamicBoneIndependantMultipleNaturalCollisionUpdateSystem")
		.WithNone<DOTSDynamicBoneCollider_BufferElement>()
		.WithNone<DOTSDynamicBone>()
		.WithBurst()
		.WithReadOnly(GetLTWR)
		.WithReadOnly(GetAnimatedLocalToWorld)
		.WithReadOnly(collisionWorld)
		.ForEach((ref DynamicBuffer<DOTSDynamicBone_BufferElement> bones, ref DynamicBuffer<Particle> particles, in IndependantDOTSDynamicBone_Tag tag, in DOTSDynamicBoneNaturalCollider_Tag tag2) =>
		{
			...
		})
		.ScheduleParallel();

```
 
 NOTE: the ```AnimtedLocalToRoot``` data isn't updated in these jobs.
 
# DOTSDynamicBoneUpdateSystem
The *DOTSDynamicBoneUpdateJob*, *DOTSDynamicBoneMultipleUpdateJob*, *IndependantDOTSDynamicBoneJob*, and the
 *DOTSDynamicBoneIndependantMultipleJob* are responsible for updating the non-collision associated ***DOT Dynamic Bone*
 Entities. The difference between them is the **DOTS Dynamic Bone** data componant.
	
The *DOTSDynamicBoneCollisionFinishJob*, *DOTSDynamicBoneMultipleCollisionFinishJob*, and the
*DOTSDynamicBoneIndependantCollisionFinishSystem* is responsible for finalizing some calculations after
another specific job is done. In an effeot to keep Jobs multithreaded, some processes needed to be broken up
in order to allow for parallel writing in burst. For example, the IndependantDOTSDynamicBone Entity can be 
seperate from the Entity it is associated with. Since Parallel writting isn't allowed on another entity,
I break up the update method and the AnimatedLocalToRoot update method into 2 Jobs to allow for parallel
writing for methods.


#Remarks

It it important to remeber that the Jobs and systems are subject to change so if you have
 any comments or suggestions, please let me know at [link](https://forum.unity.com/threads/official-dots-dynamic-bone.1194832/)