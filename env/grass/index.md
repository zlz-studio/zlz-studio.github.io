---
layout: docs
title: Grass Overview
last_modified_at: 2026-07-23
published: false
---

# Grass

## What Can the ZLZ Env Shader Grass System Do?
- Control all grass through `ZLZ_EnvDashboard` (choose the surface, pick the Grass Type, Paint, Grow) — create grass in a single click
- Grass data is stored in `ZLZ_EnvGrassData` instead of in the Scene, keeping Scenes small and letting the data travel with the Prefab — so it works in both Scene-based and Prefab-based workflows
- Grass is generated automatically from a Mask Texture; you define where grass is allowed to grow instead of painting every clump by hand
- If you want, you can still paint additional grass manually
- Supports many types of grass and flowers at the same time
- Shrinks grass along the border between grassy and non-grassy areas for a smooth transition
- Define grass-free zones through the Blocking Layer system
- Grass has an Interaction system that responds to characters
- Grass works with the `ZLZ_Global_Wind` system and sways in sync with the wind
- Grass samples its color from a camera so it always matches the ground color, and that camera is heavily optimized so it barely affects performance
- Runs on both PC and Mobile
- Includes an LOD system (LOD 0 = standard Mesh, LOD 1 = optimized Mesh, Culling = removes grass that is too far away)
- Uses GPU Instancing and is heavily optimized to squeeze out the highest possible FPS
