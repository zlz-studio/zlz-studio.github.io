---
layout: docs
title: Grass LOD
last_modified_at: 2026-07-15
published: true
---

### Grass LOD System
{% include video.html src="/env/grass/Grass_LOD_Web.mp4" %}

![Grass LOD](../Grass_LOD.png)

- Grass can be optimized at `ZLZ_Global_Grass`
- Performance displays information about the grass present in the current scene
- Users can adjust which LOD is displayed at each distance range
- LOD 0 = Standard grass Mesh
- LOD 1 = Low Mesh + reduced count
- Culled = Grass is removed
- Debug LOD Distance visualizes each distance range
- LOD 1 Density = Adjusts how much the number of grass instances within the LOD 1 range is reduced
- Includes a Distance Fade system to make LOD switching between ranges happen smoothly
