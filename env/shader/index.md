---
layout: docs
title: Environment Shader Overview
last_modified_at: 2026-07-30
published: false
---

<!-- DRAFT — ยังไม่ขึ้นเว็บจริง. พรีวิว: jekyll serve --unpublished. พร้อมขึ้นเว็บ: ลบ published: false -->

# Environment Shader

<!-- ![Env_Shader_Overall](../images/Env_Shader_Overall.webp) -->

`ZLZ_Environment_Shader` is the main surface shader of the ZLZ Env Shader package.
It renders terrain, cliffs, props, and any hand-authored mesh in your scene, while
the Grass and Water systems each run on their own dedicated shader.

- Path : `Assets/ZLZ_EnvironmentShader/Shaders/Core/ZLZ_Environment_Shader.shader`

---

## What Can the ZLZ Env Shader Do?

- Paint up to 4 decorative texture layers directly onto any mesh you authored in Maya or Blender — no Unity Terrain required
- Store paint data either in a Mask Texture or in the mesh's Vertex Colors, whichever suits your pipeline
- Project textures along world axes with Triplanar, so cliffs and blockout meshes tile without seams even when the UVs are stretched
- Break up visible texture repetition across large surfaces with Stochastic Sampling
- Settle snow, dust, or moss onto up-facing surfaces with Surface Accumulation — no extra texture needed
- Sway foliage with Wind, driven either per-material or by the scene-wide `ZLZ_Env Wind Controller` that Grass also follows
- Reflect the scene with two tiers : Reflection Probes always on, plus an optional mirror-camera Planar Reflection layered on top
- Share one RGBA Feature Mask across Metallic, Smoothness, and Emissive to save VRAM and texture samples
- Work with Unity's lighting the way an environment shader should — Lightmaps, Shadowmask, Subtractive lighting, SSAO, and URP Decals are all supported
- Share the Target Darken, Light Sweep, and Upgrade effects with ZLZ Anime Shader, so characters and environment react to the same game events
- Turn every optional feature off individually — disabled features are stripped from the compiled shader, keeping it viable on Mobile

---

## How the Feature Set Is Organised

The material inspector splits into **two groups**, the same way ZLZ Anime Shader does.

### 1. Locked Features

Core sections that are always enabled and cannot be turned off. They cost nothing extra
to keep on.

- **Rendering** — Render Queue, Blend, Cull, ZWrite, ZTest, Alpha Clipping, Cast Shadow
- **Texture** — Albedo, Tiling / Offset, and the Triplanar projection controls
- **Base Colors** — Base Color, Shadow Color, Texture Brightness
- **Lighting** — Receive Shadow, Additional Light Intensity
- **ToonRamp** — the shading ramp between lit and shadowed areas
- **Transparency** — Alpha Value and Shadow Alpha Clip

### 2. On / Off Features

Optional features toggled from the **Features** panel at the top of the material.
When one is disabled its calculations are removed from the shader entirely.

| Feature | What it does |
|---|---|
| **Paint Mode** | Blend up to 4 texture layers over the base surface |
| **Specular** | Blinn-Phong highlight with an optional toon step |
| **Reflection** | Adds the mirror-camera Planar Reflection tier |
| **Normal Map** | Surface bump detail |
| **Triplanar** | World-axis projection for UV-less or stretched meshes |
| **Stochastic** | Removes visible tiling repetition |
| **Accumulation** | Snow / dust / moss on up-facing surfaces |
| **Wind** | Vertex-stage foliage sway |
| **Emission** | Glow, with an optional pulse |
| **Target Darken** | Dims the environment to spotlight a target |
| **Light Sweep** | A band of light sweeping across the surface |
| **Upgrade** | Brightening flash for upgrade / power-up moments |

> These features are set during authoring and are not meant to be toggled at runtime.

---

## Pages in This Section

<!-- TODO: link each sub-page as it is written -->

- [Paint Mode](../shader/paint-mode/) — painting texture layers onto your own meshes
- [Triplanar](../shader/triplanar/) — world-axis projection for UV-less or stretched meshes
- [Planar Reflection](../shader/planar-reflection/) — mirror-camera reflections for flat, wet-looking surfaces
- [Normal Map](../shader/normal/) — surface relief without extra geometry, shared with SSAO and Decals
- [Specular & Metallic](../shader/specular/) — how glossy a surface is, and whether it reads as metal
- [Stochastic Sampling](../shader/stochastic-tiling/) — hides the visible repeat when a texture tiles across a large surface
- [Surface Accumulation](../shader/snow-accumulation/) — snow, dust or moss settling on up-facing surfaces, with no mask to author
