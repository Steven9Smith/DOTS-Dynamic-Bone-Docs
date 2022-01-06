---
uid: DOTSDynamicBone.Concepts.Collisions.NaturalCollisions
title: Natural Collision
---

# Natural Collisions

In **DOTS Dynamic Bone** *Natural* collision referes to collisions using Unity's Unity.Physics package components/data.
Instead of creating and storing custom data, **DOTS Dynamic Bone** uses the ```Unity.Physics.PhysicsCollider``` in its physics 
calculations. This allows a **DOTS Dynamic Bone** [Particle](xref:DOTSDynamicBone.Particle) to collide with any other 
entity that contains a ```Unity.Physics.PhysicsCollider``` and the best part is that it is very easy to use.

In order to use *Natural* collision just set the ```Use Natural Colliders``` setting to true on any 
**DOTS Dynamic Bone** Component. You can also add *Natural* collision to an already existing [Bone](xref:DOTSDynamicBone.Concepts.Bones)
by adding a ```DOTSDynamicBoneNaturalCollider_Tag``` to your *DOTSDynamicBone* ```Entity```. This tag let's the *Natural* collision
system know you want to use it. Also be sure to set UseNaturalColliders on the **DOTS Dynamic Bone** ```Entity```
to true and to make sure the ```collider``` property in the [Particle's](xref:DOTSDynamicBone.Particle) ```DynamicBuffer```
is not null. If you only want only some [Particles](xref:DOTSDynamicBone.Particle) to take part in the collision then you can leave
the [Particle's](xref:DOTSDynamicBone.Particle) collider data null which will process the [Particle](xref:DOTSDynamicBone.Particle)
noramlly.

Here's an example oh how to add a **DOTS Dynamic Bone** ```Entity``` to the *Natural* collision system:

```

public class DOTSDynamicBoneAddCollisionSystemExample : SystemBase
{
    protected override void OnUpdate()
    {

		Entities.
			WithName("AddBoneToCollisionSystemJob")
			.WithoutBurst()
			.WithAll<DOTSDynamicBone_AddToCollisionSystem_Tag>()
			.WithNone<DOTSDynamicBone.Collision.DOTSDynamicBoneNaturalCollider_Tag>()
			.ForEach((Entity e,ref DOTSDynamicBone.DOTSDynamicBone boneData,ref DynamicBuffer<DOTSDynamicBone.Particle> particles)=> {
				// set the UseNaturalColliders property to true
				boneData.UseNaturalColliders = true;

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
public struct DOTSDynamicBone_AddToCollisionSystem_Tag : IComponentData { }
public struct DOTSDynamicBone_RemoveFromCollisionSystem_Tag : IComponentData { }
```
 
