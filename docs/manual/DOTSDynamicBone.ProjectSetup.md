---
uid: DOTSDynamicBone.ProjectSetup
title: Project Setup
---

#Prerequisites

**NOTE: THIS WILL NOT WORK IN A NON-DOTS/ECS ENVIROMENT. YOU MUST BE WORKING IN UNITY DOTS & ECS**

Before you setup **DOTS Dynamic Bone** it is **important** that you import the following unity
packages from the *Package Manager* (Note: links may not always send you to the latest version):

- [Unity.Animation v0.9.0-preview.6](https://docs.unity3d.com/Packages/com.unity.animation@0.9/manual/index.html)
- [Unity.Entities v0.17.0-preview.42](https://docs.unity3d.com/Packages/com.unity.entities@0.17/manual/index.html)
- [Unity.Physics v0.6.0-preview.3](https://docs.unity3d.com/Packages/com.unity.physics@0.6/manual/index.html)
- [Unity.Rendering.Hybrid v0.11.0-preview.44](https://docs.unity3d.com/Packages/com.unity.rendering.hybrid@0.11/manual/index.html)
- Either the [Universal Render Pipeline](https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@11.0/manual/index.html
) and/or [High Definition Render Pipeline](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@11.0/manual/index.html).

#Setup

After adding the required packages you will need to add these scripting defines to your project

ENABLE_COMPUTE_DEFORMATIONS

and is recommended you add these

ENABLE_HYBRID_RENDERER_V2
UNITY_POST_PROCESSING_STACK_V2

That's it!
