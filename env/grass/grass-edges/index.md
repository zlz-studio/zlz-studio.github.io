---
layout: docs
title: Grass Edges
last_modified_at: 2026-07-23
published: false
---

# Grass Edges

{% include video.html src="/env/grass/Grass_Interaction_Web.mp4" %}

![Grass LOD](../Grass_LOD.png)

The core idea: grass can be kept from growing in three places — objects in the scene, the mesh edge, and areas painted over with another texture/color
and instead of cutting the grass on a hard straight line, all three use the same technique: **gradually shrinking the grass shorter until it fades out as it nears the edge**
so the transition looks smooth and natural with no sharp edges.

## Kept Out by Three Sources

**1. Objects in the scene — Blocking Layers**
Set on the `ZLZ Env Dashboard`, in the Plant Area section.
- **Blocking Layers** — choose the Layers of the objects that keep grass out (only those with a Collider), such as a house, a rock, or a crate. Nothing = grass ignores objects entirely. Sampled at Grow, so Re-grow to apply
- **Keep Off Objects (m)** — how far grass stays away from those objects, tuned live in real time (0–2 m). Higher widens the bald ring around each object

**2. Mesh edge — Keep Off Mesh Edge**
Set per Grass Type (on the Grass Type card's Edge section).
- **Keep Off Mesh Edge (m)** — how far grass stays in from the surface edge, so blades don't poke past it. 0 = off

**3. Areas with another texture/color — Keep Off Paint**
Set per Grass Type (on the Grass Type card's Edge section).
- **Keep Off Paint (m)** — how far grass stays away from the painted ground. 0 = stops exactly where the paint takes over

## The Shared Technique: Shrink Near Edges
- **Shrink Near Edges (m)** — the last stretch (in metres) over which blades taper down instead of ending on a hard line. It applies to every edge at once (paint, mesh edge, objects, and erased holes). 0 = hard cut

This is the heart of the system — it makes all three cases above blend the same, smooth way.

## Debug
- **Debug Grass Area** — toggles an overlay to check the result: green = full grass, amber = inside the Shrink zone (grass, but shorter), red = no grass
