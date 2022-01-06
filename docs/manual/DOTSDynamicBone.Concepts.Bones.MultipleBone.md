---
uid: DOTSDynamicBone.Concepts.Bones.MultipleBone
title: Multiple Bone
---

# Multiple Bone

In **DOTS Dynamic Bone** a *Multple Bone* is associated with entities that contain the 
[DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement)
```DynamicBuffer```. The [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement) data can be modified at any time. 

The [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement) contains all the 
[DOTSDynamicBone](xref:DOTSDynamicBone.DOTSDynamicBone) data as well as 2 ```int``` value that represents a start and end index respectivly.

the start and end index data is used to get the relevant [Particle](xref:DOTSDynamicBone.Particle) data within the 
```DynamicBuffer<Particle>```. So to access specific [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement)
data's [Particle](xref:DOTSDynamicBone.Particle) data you would have to do something similar to this:
```
	...
	DynamicBuffer<DOTSDynamicBone_BufferElement> bones = GetBuffer<DOTSDynamicBone_BufferElement>(RigComponentEntity);
	DynamicBuffer<Particle> particles = GetBuffer<Particle>(RigComponentEntity);
	for(int i = 0; i < bones.Length; i++){
		DOTSDynamicBone_BufferElement bone = bones[i];
		DOTSDynamicBone boneData = bone.Value;
		for(int j = bone.start; j < bone.end; j++){
			// deal with particle stuff in here
		}
	}
	...
```

Please checkout the [DOTSDynamicBone_BufferElement](xref:DOTSDynamicBone.DOTSDynamicBone_BufferElement) for more information
