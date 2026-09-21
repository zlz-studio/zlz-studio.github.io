---
layout: docs
title: v1.0.0 — Initial Release
last_modified_at: 2026-09-21
---

[&larr; All versions](/env/changelog/)

# v1.0.0 — Initial Release

**Released:** September 21, 2026

Initial release of **ZLZ Env Shader** for the Universal Render Pipeline (URP) — an environment toolkit of surface, grass and water shaders built to sit alongside ZLZ Anime Shader.

### Features — Environment Shader

- **Paint Mode** : Blend up to 4 surface layers by painting, with custom brush support
- **Triplanar** : UV-free texture mapping with no stretching on cliffs or sloped ground
- **Planar Reflection** : True reflections from real scene geometry, with adjustable blur, distortion and layer filtering
- **Normal Map** : Normal map support for added surface detail without extra polygons
- **Specular & Metallic** : Control over glossiness and metalness, tuned for anime-style art
- **Stochastic Sampling** : Breaks up visible texture tiling across large areas
- **Surface Accumulation** : Adds snow, sand or dust layers that build up based on surface direction
- **Wind** : Wind system for leaves and foliage, driven from a single global controller
- **Emission** : Controls emissive color and intensity for glowing objects in the scene
- **Fog** : Stylized height fog blended with the sky, adding depth to every shot

### Features — Grass

- **Grass Grow** : One click grows grass across the whole scene, following a mask texture
- **Grass Paint Brush** : Add or remove grass by hand in the Scene view, with the automatic placement left intact
- **Grass Material** : Grass materials with presets for color, height and blade shape
- **Grass Global Wind** : One global wind drives every blade across the map in the same direction
- **Grass Optimized** : GPU-instanced rendering with every blade sharing one mesh
- **Grass Color Camera** : Grass picks up the ground color beneath it to blend with the terrain
- **Grass Interaction** : Blades bend and part around characters or objects moving through them
- **Grass LOD** : Distance-based detail reduction to hold framerate in large scenes
- **Grass Edges** : Define grass boundaries to carve paths and clearings without erasing blade by blade
- **Grass Resolution** : Renders grass color at half or quarter resolution to cut overdraw
- **Grass Mesh Baker** : Bakes a grass texture into a real silhouette mesh

### Features — Water

- **Water Interaction** : Ripples that trail characters or objects moving across the surface
- **Water Waves** : Surface waves with adjustable direction, height and frequency
- **Water Floater** : Floating objects that rise, fall and tilt with the real wave motion
- **Water Foam** : Foam along shorelines and around objects moving through the water
- **Water Ring Wave** : Expanding ring waves from impact points, for objects dropped in or hits landing
- **Water Sparkle** : Shimmering light on the water surface for an anime look
- **Water Caustics** : Animated refracted light patterns projected onto the underwater floor
- **Underwater** : A full underwater view with depth-based fog and a seamless above/below surface transition

### Compatibility

- Built for the Universal Render Pipeline (URP)
- Supports Unity 2022.3 LTS and Unity 6 (6000.3)
