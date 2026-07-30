---
layout: docs
title: Water Caustics
last_modified_at: 2026-07-30
published: true
---

# Water Caustics

The rippling web of light on the floor beneath clear water — sunlight focused by the moving surface into bright threads that weave together and crawl, endlessly. It is the single cue that tells a player at a glance "this water is clear and there is a bottom down there", instead of a blue sheet laid over the sand.

The pattern is **projected onto the floor**, not stuck to the surface: it is computed from the real world position of the bottom seen through the water, so wherever the camera moves the web stays with the ground it belongs to, and the ripples slide across it rather than dragging it along.

All of it is procedural — no textures, no extra pass, and nothing at all while the feature is off.

## Showcase Water Caustics
{% include youtube-loop.html id="LgNnoRAp2yA" %}

---

## Setup

1. **On the water material** — turn on the **Caustics** feature (the button grid at the top of the Inspector)
2. **On the URP Asset** — make sure **Depth Texture** is on, because the system has to know how deep the floor is before it can project the web onto it

> **Pair it with Refraction.** With Refraction on, the water shows the real bottom through the surface and the caustics land exactly on that visible bottom — the setup this was built for. Without Refraction the web still works: the surface's own opacity is raised by the web's brightness for you, so it punches through the clear shallows.

Nothing to bake, no component to add, no extra light to place — the web takes its colour and its light from the scene's Directional Light.

---

## Parameters

![Material_WaterCaustics](../water-caustics/Material_WaterCaustics.png)

All of these live on the material under **Caustics**, and only appear once the feature is on.

- **Color** (HDR, default white) — the colour of the light web. It is already multiplied by the Directional Light's colour, so white is the normal answer; reach for this when the light under water should read greener or bluer than the light above it
- **Intensity** (`0–4`, default `1.2`) — how bright the web is. `0` = invisible
- **Scale** (`0.1–8`, default `1`) — how fine the web is, measured in **world space** rather than UV or mesh size. High = a tight small mesh (a swimming pool), low = a broad one (open sea). Stretch the water body as far as you like and the web keeps its size
- **Speed** (`0–3`, default `0.6`) — how fast the web crawls. `0` = frozen (still a web, just not moving)
- **Sharpness** (`1–16`, default `6`) — how thin the threads are. High = fine crisp lines, like very clear pool water, and a darker floor overall because the threads are thinner. Low = wide soft light spread over the whole bottom
- **Depth Fade** (`0.1–20`, default `4`) — how many metres of water column it takes for the web to disappear. It is brightest at the shore and falls off on an accelerating (squared) curve with depth

> **Why does it have to fade with depth?** Two reasons that match the real thing: the focused light is absorbed before it reaches a deep bottom, and deep water in this system is already opaque (see Depth Color). Set Depth Fade near the **See-Through Depth** in the Depth Color section and the web will vanish right about where the bottom stops being visible.

---

## Where the Pattern Comes From

The web is built from **two layers of Worley noise** at different scales and phases, combined. Each cell owns a feature point that orbits inside it on a sine, which is what makes the whole field crawl and shimmer. The bright part is the **ridge between cells** — the ground that is furthest from any feature point — and that ridge network is the web you see.

Two layers, because a single one reads immediately as one repeating round grid.

### The Far Field Does Not Go Blank

Look far enough away and the threads become finer than a pixel. At that point the system stops drawing threads and uses **the pattern's own average brightness** instead — not zero. So the distant floor keeps its caustic light and loses only the detail, rather than ending on a visible line where the effect stops.

This matters most for a camera close to the bottom (while diving, for instance): the view grazes the floor, so "finer than a pixel" arrives within a few metres instead of tens of them.

---

## Works With Other Features

### Underwater
Both sides run the **same pattern, literally** — the pattern code lives in one shared file that both paths call. Dive under and the web on the floor continues exactly as it read from above the surface, rather than being a second pattern tuned to look similar.

This Caustics section owns **the entire look** (Scale / Speed / Sharpness / Colour / Depth Fade / Intensity). The Underwater side only adds two brightness multipliers:

- **Underwater Fog > Floor Caustics** — a multiplier on Intensity for the floor seen while diving (`1` = as bright as the view from above shows it)
- **Material > Underwater > Surface Caustics** — the web playing on the surface itself when you look up from below

**Switch the Caustics feature off and both of those go dark with it**, and the Surface Caustics slider greys out. See [Underwater]({{ '/env/water/water-underwater/' | relative_url }}).

### Refraction
The partner feature, and there is one detail already handled for you: the depth fade reads the **refracted** depth rather than the true one. So the bright patch of web over a submerged prop bends along with that prop's refracted image, instead of printing the prop's real silhouette on top as a second, rigid layer.

### Depth Color
This is what decides how far down the water stays clear, so keep the Caustics **Depth Fade** in step with **See-Through Depth** — otherwise you get a bright web dancing on a bottom nobody can see any more (or a clearly visible bottom with no web on it).

### Lighting — Receive Shadow
The web is multiplied by the shadow the surface receives, so **the shadow of a rock or a tree falling across the water puts out the web underneath it**, like a cloud crossing the sun. And because it is multiplied by the Directional Light's colour, it dies on its own at night.

### Sparkle
Different layers that complement each other: Sparkle is glitter on **the surface**, Caustics is light on **the floor**. Both keep themselves to shallow water on the same principle, and both calm their far field the same way. Run them together and shallow water is alive above and below the surface at once.

### Reflection
Caustics is added **before** the specular and the reflection, on purpose — it is light on the bottom, so where the surface turns into a hard mirror it should be hidden by the mirror image. That falls out of this ordering by itself.

### Waves / Interaction
The web is glued to the floor and does not ride the surface normal, so ripples and the rings a character leaves **do not drag it around**. With Refraction on, though, the view of the bottom (web included) already wobbles with the refraction — which is the correct behaviour.

---

## Debug

The **Debug** section at the bottom of the Inspector has a **Caustics** mode that shows the web's raw coverage (after the depth and distance fades, before colour and Intensity). Use it when the web will not appear: if this mode is black across the screen, the problem is depth or the Depth Texture, not the colour or the brightness.

---

## Good to Know

- **It projects straight down**, so the web drapes over everything submerged rather than a flat bottom only — rocks, posts and props get dappled too, which is exactly what light does under real water
- **No textures at all.** Everything is computed live in the pixel: no files to manage, no extra VRAM
- **Cheaper than it looks.** Pixels deeper than Depth Fade, or too far away for the threads to resolve, leave the calculation before the expensive part (the Worley loops), so the real cost sits only on the band of shallow water that actually shows the web
- **Rock steady under TAA.** The floor position is rebuilt in a way that is immune to the camera's jitter, so the web never shakes
- **It animates in Edit Mode too**, on the shader's own clock — tune a value and the web crawls straight away, no need to press Play
- **Depth Texture is required.** With it off there is no floor data to project onto
- The look belongs to the **material**, so every pond sharing one material gets the same web. For a pool with a finer web than the sea, give it its own material
