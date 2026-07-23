---
layout: docs
title: Grass Interaction
last_modified_at: 2026-07-23
published: true
---

# Grass Interaction

Grass reacts to movement: nearby blades bend away from anything that passes through them and return once it leaves. It runs on the GPU with no per-blade colliders, so it stays cheap even on a large instanced field.

## Showcase
{% include video.html src="/env/grass/Grass_Interaction_Web.mp4" %}

Grass responds when a character or object moves through it.

### Interactor (per object)
Drop the `ZLZ_Env Grass Interactor` component on anything that should part the grass — the player, an NPC, a rolling prop.

- Add Component ▸ `ZLZ / Environment Shader / ZLZ_Env Grass Interactor`
- **Radius** — the area (in metres) around the object within which grass bends away. Size it to the object's footprint; it shows as a wire sphere in the Scene view when the object is selected
- Up to 8 interactors affect the grass at once. If a scene has more, the ones nearest the camera win

### Strength (grass material)
How hard the grass reacts is set on the grass material, in the **Interaction** section (keep the Interaction toggle on):

- **Push** — how far the blade tip slides away from an interactor
- **Flatten** — how hard the blade is pressed down inside an interactor's radius

### Debug
- Set the material's **Debug** view to **Interaction** to see where interactors are affecting the grass
