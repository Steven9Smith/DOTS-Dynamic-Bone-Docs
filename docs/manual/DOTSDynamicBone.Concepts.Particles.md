---
uid: DOTSDynamicBone.Concepts.Particles
title: Particles
---

# What is a Particle?

[Particles](xref:DOTSDynamicBone.Particle) in **DOTS Dynamic Bone** is a data structure that 
represents a Bone within a model i.e: a Hip, Finger, Hair, or Tail bone. The [Particles](xref:DOTSDynamicBone.Particle) are updated 
and modified during the simulation based on the [Particle](xref:DOTSDynamicBone.Particle) and [DOTSDynamicBone](xref:DOTSDynamicBone.
DOTSDynamicBone) parameters. [Particles](xref:DOTSDynamicBone.Particle) also can represent different things like  segments, or divisions 
within a model. During each update, data from the associated [DOTSDynamicBone](xref:DOTSDynamicBone.DOTSDynamicBone) is used to update 
the [Particles](xref:DOTSDynamicBone.Particle) in an initalization-like step. Afterward, the [Particles](xref:DOTSDynamicBone.Particle) 
is put through different methods that update its position and rotation based on the input data.

# Where is a Particle Created?

A [Particle](xref:DOTSDynamicBone.Particle) is created during the ```Convert()``` call in a *DOT Dynamic Bone Component*. You also 
have the option to create one yourself using one of the static methods within the [Particle](xref:DOTSDynamicBone.Particle) struct.
Example #1:
```
// This is an example on how to add a particle from a MonoBehaviour.
// NOTE: DOTS Dynamic Bone systems also require a DOTSDynamicBone or a
//		 DOTSDynamicBone_BufferElement to be present as well otherwise,
//       the particle's data won't be processed or updated.
public class CreateParticleComponent : MonoBehaviour, IConvertGameObjectToEntity{
	
	public Transform TailBone;
	// Optional boolean value
	public bool m_ExcludeParticleFromCalculations = false;
	// Optional boolean value
	public bool m_ExcludeParticleFromCollision = false;

	public void start(){...}
	public void onValidate(){...}
	
	/// <summary>
		/// Sets up the Entity during conversion
		/// </summary>
		/// <param name="entity">The entity the GameObject will be convert to. </param>
		/// <param name="dstManager">The provided Entitymanager for conversion. </param>
		/// <param name="conversionSystem">The provided GameObjectConversionSystem for conversion. </param>
		public void Convert(Entity entity, EntityManager dstManager, GameObjectConversionSystem conversionSystem){

			DynamicBuffer<Particle> m_Particles = dstManager.AddBuffer<Particle>(entity);

			m_Particles.Add(
				Particle.Create(
					m_ConversionSystem,
					TailBone,
					m_ExcludeParticleFromCalculations,
					m_ExcludeFromCollision
				)
			);
		}

}
```
Example #2
```
// this is an example on how to add a particle if you alread have the Particle's Entity but did not add it to an Entity with
// either a DOTSDynamicBone or a DOTSDynamicBone_BufferElement
	...
	DynamicBuffer<Particle> m_Particles = GetBufferFromEntity<Particle>(DOTSDynamicBoneEntity);
	
	m_Particles.Add(
		Particle.Create(
			m_ParticleEnity,
			new ParticleTransform(
				position,
				localPosition,
				rotation,
				localRotation,
				localScale,
				lossyScale, 
				childCount
			)
		)
	);
	...
```

Please [Check Out the Particle Scripting Api Page](xref:DOTSDynamicBone.Particle) for more information on the ```Create``` method.

# Can I Modify a Particle?

Yes, you can modify [Particles](xref:DOTSDynamicBone.Particle) during runtime by modifying the ```DynamicBuffer```!
Here's an example on how to modify a [Particle](xref:DOTSDynamicBone.Particle):
```
	...
	DynamicBuffer<Particle> m_Particles = GetBufferFromEntity<Particle>(DOTSDynamicBoneEntity);
	
	int lastParticleIndex = m_Particles.Length-1;
	
	Particle particle = m_Particles[lastParticleIndex];
	// now we modify the particles properties here.
	// some properties like disable and exclude require special functions to deal with.
	// this is because positions and rotations still need to be calculated otherwise other 
	// particles may not look right and may experience "much jank". 
	
	// when you call Disable(), the last values of m_Stiffness, m_Inert,
	// m_Damping, m_Friction, and m_Elasticity are stored in the Particle struct. Then 
	// when you call Enable() those stored values are restored.
	particle.Disable();
	
	particle.m_Radius *= 2; //multiply Particle radius by 2 
	
	 // it's important to set the value of the buffer otherwise the changes won't take effect!
	m_Particles[i] = particle;
	...
```

# Particle Property Overview

Here I will go over some important properties of [Particle](xref:DOTSDynamicBone.Particle). NOTE: Any properties not covered
can be found [here](xref:DOTSDynamicBone.Particle). 

## Entity m_TransformEntity

m_TransformEntity is the Entity the [Particle](xref:DOTSDynamicBone.Particle) is associated with. This is a required field 
during initialization and is recommended you don't modify this after initialization unless you know what you were doing.
However, this field is required when your desired RigComponentEntity has the 
[DOTSDynamicBoneParticleLocalToWorldOverride_Tag](xref:DOTSDynamicBone.DOTSDynamicBoneParticleLocalToWorldOverride_Tag).

## ParticleTransform m_Transform

m_Transform is a [ParticleTransform](xref:DOTSDynamicBone.ParticleTransform) that represents the 
[Particle's](xref:DOTSDynamicBone.Particle) transform component. since the ```RigidTransform``` didn't come with localPosition,
 local Rotation, localScale, and lossyScale, I made [ParticleTransform](xref:ParticleTransform). the transform is a public struct
 so you can create/initialize a [ParticleTransform](xref:ParticleTransform) at any time.
 
## AnimatedLocalToRootIndex

AnimatedLocalToRootIndex is an ```int``` that represents the index in the ```DynamicBuffer<Unity.Animation.AnimatedLocalToRoot>```
that is attached to the **RigComponentEntity**.

## m_ParentIndex

m_ParentIndex is the index of the [Particle's](xref:DOTSDynamicBone.Particle) parent within the 
```DynamicBuffer<Particle>``` buffer. If the m_ParentIndex is ```-1``` then it is classified as a *Root*
Bone/Particle.

## m_Radius

m_Radius is the collision radius of a [Particle](xref:DOTSDynamicBone.Particle) that is used for the 
 [Preset Collision](xref:DOTSDynamicBone.Concepts.Collisions.PresetCollisions) collision system. 

## m_BoneLength

the length of this [Particle](xref:DOTSDynamicBone.Particle) in relation to the associated Bone's total bone length.

## collider

This is the Unity.Physics.PhysicCollider associated with the m_TransformEntity. If you are not using the Natural Colllision System
then this value will be null.





 
