---
uid: DOTSDynamicBone.Concepts.Bones
title: Bones
---

# Bones

A **Bone** is a data structure that holds all data relevant to their associated [Particles](xref:DOTSDynamicBone.Particle) 
and *Collider(s)*.the **Bone** data structure is associated with 1 ```Entity```. The ````Entity``` the **Bone** is associated with
is refered as a **RigComponentEntity**. To create a **RigComponentEntity**, all you have to do is add a RigComponent and a ConvertToEntity
Component to a GameObject. This will add all nessacary RigComponent data furing the Coversion of the GameObject.

Each **Bone** is assigned a specific ```DynamicBuffer<Particle>```. 
NOTE: depending on the **Bone** type the way to access the **Bone's** ```DynamicBuffer<Particle``` data may be different.

There are 2 types of *Bones* that the system contains. They are:
- [Single Bone](xref:DOTSDynamicBone.Concepts.Bones.SingleBone): where a [DOTSDynamicBone](xref:DOTSDynamicBone.DOTSDynamicBone) data structure added to an ```Entity```. The 
[DOTSDynamicBone](xref:DOTSDynamicBone.DOTSDynamicBone) data structure only represents a single bone.
- [Multiple Bones](xref:DOTSDynamicBone.Concepts.Bones.MultipleBone): where a [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement) data structure
is added to an ```Entity```. Unlike Single Bone, Multiple Bones is for use with Multiple Bones on a single ```Entity```.

## Why not just use DynamicBuffer for both cases?

I do this simply for efficency and unnessacary memory allocation. The ```DynamicBuffer``` uses indexing and in order to manage
multiple [Particles](xref:DOTSDynamicBone.Particle) on a single Entity, some more data needed to be added in the
[DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement) data structure.

## Can I modify the Bone data?

Yes, you can modify and create new bone at runtime in order to change how the **DOTS Dynamic Bone** functions. 
See [Single Bone](xref:DOTSDynamicBone.Concepts.Bones.SingleBone) and [Multiple Bones](xref:DOTSDynamicBone.Concepts.Bones.MultipleBone)
for more information.