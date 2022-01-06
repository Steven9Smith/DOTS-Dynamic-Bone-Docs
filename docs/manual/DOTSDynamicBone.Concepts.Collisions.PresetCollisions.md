---
uid: DOTSDynamicBone.Concepts.Collisions.PresetCollisions
title: Preset Collisions
---

# Preset Collisions

In **DOTS Dynamic Bone** the *Preset Collision System* simpily refers to colliders generated with/within the [DOTSDynamicBoneCollider](xref:DOTSDynamicBone.Collision.DOTSDynamicBoneCollider) Component. 
The reason for this system is to allow the developer to either define a specific set of Entities to collide with or to collide with simple some simple shape data. When you add the 
[DOTSDynamicBoneCollider](xref:DOTSDynamicBone.Collision.DOTSDynamicBoneCollider) Component it adds a ```DynamicBuffer<DOTSDynamicBoneCollider_BufferElement>``` which holds the data retaining to your *Preset Colliders*.
This data is used everyframe to perform come collision calculations.


Here's an example oh how to add a **DOTS Dynamic Bone** ```Entity``` to the *Preset* collision system:

```

public class DOTSDynamicBoneAddToPresetCollisionSystemExample : SystemBase
{
    protected override void OnUpdate()
    {
		// Get Collider Entity
		...
		Entity colliderEntity;
		Unity.Physics.PhysicsCollider otherEntityCollider = GetComponentData<Unity.Physics.PhysicsCollider>(colliderEntity);
		
		...
	

		Entities.
			WithName("AddBoneToPresetCollisionSystemJob")
			.WithoutBurst()
			.WithAll<DOTSDynamicBone_AddToPresetCollisionSystem_Tag>()
			.WithNone<DOTSDynamicBone.Collision.DOTSDynamicBoneNaturalCollider_Tag>()
			.WithReadOnly(colliderEntity)
			.WithReadOnly(otherEntityCollider)
			.ForEach((Entity e,ref DOTSDynamicBone.DOTSDynamicBone boneData,ref DynamicBuffer<DOTSDynamicBone.Particle> particles)=> {
				// set the UseNaturalColliders property to true to use the Particle's PhysicsCollider
				boneData.UseNaturalColliders = true;
				// add the buffer. NOTE: this should invalidate the DynamicBuffer<DOTSDynamicBone.Particle> buffer so we have to get it again
				DynamicBuffer<DOTSDynamicBoneCollider_BufferElement> m_Colliders = AddBuffer<DOTSDynamicBoneCollider_BufferElement>(e);
				particles = GetBuffer<DOTSDynamicBone.Particle>(e);
				
				//now let's add a preset collider
				m_Colliders.add(new DOTSDynamicBoneCollider_BufferElement
					{
						m_Direction = TransformsExtensions.Direction.Y,
						enabled = true,
						fallbackToParicleRadiusOnNullPhysicsCollider = true,
						entity = colliderEntity,
						m_Radius = 0.5f,
						boneEntity = e,
						transform = new ParticleTransform(), //<- you may want to set this yourself
						useUnityPhysicsCollider = true,
						useParticlePhysicsCollider = true,
						m_Bound = TransformsExtensions.Bound.Outside,
						m_Center = new float3(),
						persistant = true,
						colliderPtr = otherEntityCollider
					});
				
				// let's add the Unity.Physics.PhysicsCollider to each respective particle.
				// let's also disable collision calculations for every Particle who ith index is divisiable by 2 just because we can. 
				for(int i = 0; i < particles.Length; i++)
                {
					DOTSDynamicBone.Particle p = particles[i];

					p.m_ExcludeFromCollision = i % 2 == 0;  // exclude from collision if i mod 2 == 0

					// if the m_TransformEntity has a PhysicsCollider then set it
					unsafe
					{
						if (HasComponent<Unity.Physics.PhysicsCollider>(p.m_TransformEntity))
						{
							Unity.Physics.PhysicsCollider col = GetComponent<Unity.Physics.PhysicsCollider>(p.m_TransformEntity);

							if (col.IsValid && col.Value.IsCreated)
								p.collider = col.ColliderPtr;
							else p.collider = null;
						}
						else p.collider = null;
					}

					particles[i] = p;
				}

				// these two functions are not Burstable. this addition and removal should be done
				// elsewhere. This is just an example.
				EntityManager.AddComponentData(e, new DOTSDynamicBone.Collision.DOTSDynamicBoneNaturalCollider_Tag { });
				EntityManager.RemoveComponent<DOTSDynamicBone_AddToCollisionSystem_Tag>(e);

			})
			.Run();

    }
}
public struct DOTSDynamicBone_AddToPresetCollisionSystem_Tag : IComponentData { }
public struct DOTSDynamicBone_RemoveFromPresetCollisionSystem_Tag : IComponentData { }
```
 


[]


This Component has the data nessacary to perform wither a *Natural Collision* or a 
