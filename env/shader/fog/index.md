---
layout: docs
title: Fog
last_modified_at: 2026-08-03
published: false
---

<!-- DRAFT — ยังไม่ขึ้นเว็บจริง. พรีวิว: jekyll serve --unpublished. พร้อมขึ้นเว็บ: ลบ published: false -->

# Fog

**One atmosphere for the whole scene.** Distance fades the world into a haze, low ground mist pools in the valleys while the hills rise out of it, and the skybox picks up the same tone at the horizon so land and sky meet with no seam.

Unlike the rest of the Environment Shader section, **fog is not a material feature**. There is no toggle in the Features grid and nothing per-material to tune — one component in the scene owns every setting, and everything that draws fog reads from it. Ground, cliffs, props, grass and water all sink into the same haze because they are all fogged by the same numbers.

## Showcase Fog

{% comment %} TODO: ใส่คลิปโชว์ฟีเจอร์ — include youtube-loop.html พร้อม id เมื่ออัดวิดีโอเสร็จ {% endcomment %}

---

## Concept — Two Halves, One Formula

The fog is drawn in two places, and knowing which is which explains everything else on this page.

| Half | What it covers | How |
|---|---|---|
| **Fullscreen pass** | the opaque scene **and the skybox** | the `ZLZ Env Fog` Renderer Feature reads the depth texture and blends fog over the camera |
| **In the water shader** | the ZLZ water surface | the water applies the **same** formula to its own finished colour |

Why the split: a transparent surface is **invisible to the depth texture**, so the fullscreen pass simply cannot see the water. Rather than leave a crisp lake sitting in front of a fogged valley, the water shader includes the same fog code and fogs itself.

Both halves come from one file, so there is no second copy of the maths to drift out of sync — the surface and the world behind it always land on the same tone.

> **The pass runs *before* transparents.** That is what lets the water draw crisp on top and fog itself, and it also means the water's refraction sees the scene fogged **exactly once** rather than hazing twice where you look through it.

---

## Setup

Two things, and the second one is usually already done for you.

### 1. The component in the scene

Add **`ZLZ_Env Fog`** from `Add Component > ZLZ > Environment Shader > ZLZ_Env Fog`. It can live anywhere — an empty `Fog` object, or the same object as the Environment Dashboard.

Or let the setup action do it: **`GameObject > ZLZ > Setup ZLZ Global`** creates the Grass, Wind **and** Fog globals together under a shared `ZLZ_Global` parent. It is idempotent — run it on an older scene and it only adds the pieces that are missing.

> **Nothing is baked into the scene.** Every value lives on the component, so the fog rides Prefabs like any other component and a level can ship its own atmosphere with it.

### 2. The Renderer Feature

**`ZLZ Env Fog`** has to be in the URP Renderer's feature list. It is installed automatically when the package is imported, alongside the other ZLZ features — if it ever goes missing, add it by hand with **Add Renderer Feature** on the URP Renderer.

The feature itself **has nothing to tune**. Every setting is on the scene component.

> **No URP asset checkbox to tick.** The pass requests the depth texture itself, so Depth Texture does not have to be enabled on the URP Asset for the fog to work.

### Turning it off

- **Set Intensity to `0`**, or **disable the component**, or **delete it** — all three do the same thing: the pass is never enqueued and every consumer skips its work

An unused fog costs nothing at all, so there is no reason to strip the component out of a scene that simply does not want fog today.

---

## Parameters

<!-- TODO: ![Component_Fog](../fog/Component_Fog.png) -->

### Fog

- **Fog Color** (default a pale sky blue) — the colour everything fades into. Something close to your horizon sky gives the classic soft look; a darker, dirtier tone reads as haze or dusk
- **Intensity** (`0–1`, default `1`) — master strength, and the master switch. `0` turns the **whole system** off and skips all fog work everywhere
- **Start Distance** (m, default `40`) — where the fog begins. Nothing closer than this is touched
- **End Distance** (m, default `250`) — where the fog reaches full strength
- **Density Curve** (`0.25–4`, default `1`) — reshapes the density between Start and End **without moving either end**. Below `1` the fog fills in early (thicker up close, the far end unchanged); above `1` it stays clear longer and thickens late. `1` = an even ramp

> **End is kept above Start for you.** Drag End below Start and it snaps back to Start + 0.1 m, so the band can never invert.

### Height

- **Height** (world Y, default `0`) — at and below this height the fog is at full strength
- **Height Fade** (m, default `200`) — how many metres above Height the fog takes to thin away to nothing

**This pair is what turns distance fog into weather.** With the default large fade the height band is ~1 everywhere and you get plain distance fog. Shrink the fade to a few metres and you get **a low mist sitting on the ground** that hills, towers and rooftops rise out of.

### Sky

- **Sky Fog** (`0–1`, default `1`) — how much fog the **skybox** picks up at the horizon. `1` blends land and sky seamlessly; `0` leaves the skybox untouched

Sky pixels have nothing behind them, so distance is meaningless there — the **view direction** decides instead. Fog gathers at the horizon and clears toward the zenith, and at the horizon it reaches exactly the strength the ground fog reaches at End Distance. That is why the far hills and the sky behind them meet with no visible line.

### Noise

Off by default. Turn it on and the fog **gathers and thins in slow moving banks** instead of sitting perfectly even — the difference between "a fog setting" and "weather".

- **Noise** (default off) — with it off the noise maths is skipped entirely, not just hidden
- **Noise Amount** (`0–1`, default `0.5`) — how far the drift departs from the flat density. `1` lets it wander a full ±50%
- **Noise Scale** (default `0.015`) — world frequency of the drift. **Smaller = larger, calmer banks**
- **Noise Speed** (default `0.3`) — how fast the banks drift

The banks travel diagonally rather than pulsing in place, so the motion reads as weather rather than as a breathing texture. They also **settle back to flat fog in the distance** — from halfway to End Distance the drift fades out, because out there the pattern is smaller than a pixel and could only shimmer, and a far fog wall should sit calm anyway.

### Compatibility

- **Sync Unity Fog** (default off) — see below

---

## Sync Unity Fog

The ZLZ fog covers **the opaque scene, the skybox and the ZLZ water**. It cannot cover a particle system or a third-party standard shader, because those are transparent and never reach the depth texture.

Turn this on and, **while playing**, the component drives Unity's own built-in fog with matching values — colour, Linear mode, and the same start distance — so those materials fade toward the same tone instead of hanging bright in front of a fogged valley.

> **The trade-off, stated plainly.** The ZLZ ground and grass shaders already apply Unity fog themselves. With this on they are fogged twice inside the blend band, which reads as a slightly denser fog than the sliders say. That is why it is **off by default** — turn it on when you have particles or outside materials that need to match, then re-tune Start / End with it on.

Two things it deliberately does not do:

- **Edit mode is never touched.** Writing `RenderSettings` on every slider drag would dirty the scene, so the bridge only runs in play mode
- **Your own fog setup survives.** The project's previous fog settings are captured once and restored the moment this stops driving

---

## Works With Other Features

### Ground, Cliffs, Props and Grass
Nothing to enable. They are opaque, so the fullscreen pass fogs them along with everything else in the depth texture — including grass, which is alpha-tested and writes depth like any solid surface.

### Water
The water fogs itself with the same formula, applied to its **finished** colour — reflection, foam and caustics included — so the lake and the shore behind it meet in one haze. See [Water Waves]({{ '/env/water/water-waves/' | relative_url }}).

### Planar Reflection
No extra pass needed. A reflective floor is opaque, so the pass fogs it together with its reflection, and the water fogs its own final colour with the reflection already in it. See [Planar Reflection]({{ '/env/shader/planar-reflection/' | relative_url }}).

### Underwater
A separate system with its own murk, tuned per pond. The two do not switch each other off, but in practice the environment fog contributes almost nothing below the surface — the distances involved down there are far shorter than Start Distance. See [Underwater]({{ '/env/water/water-underwater/' | relative_url }}).

### Target Darken
Independent, and they layer in the sensible order — the darken dims the surfaces, the fog then hazes the dimmed result. See [Target Darken FX Runtime]({{ '/env/fx/target-darken/' | relative_url }}).

---

## Performance

- **Off is genuinely free.** No component, a disabled one, or Intensity `0` means the pass is never enqueued and the water pays a single uniform compare
- **One fullscreen triangle, no colour copy.** The pass outputs (fog colour, amount) and lets alpha blending do the mix, rather than reading the camera colour back to blend it in a shader
- **The noise is pure maths, no texture.** With Noise off it is skipped on a uniform branch, so the toggle really removes the cost rather than just the look
- **The settings are published only when they change.** The component compares a hash each frame instead of pushing ten shader globals blindly

---

## Limits

- **VR is not supported yet** — a fullscreen triangle under stereo needs its own path, so while XR is enabled the pass switches itself off. The in-shader water fog still runs, so water in VR scenes stays consistent with itself
- **Transparent materials are not fogged**, apart from the ZLZ water. Glass, particles and outside transparent shaders never enter the depth texture — **Sync Unity Fog** is the answer for those
- **Cameras rendering to a Render Texture are skipped**, as are preview cameras, reflection probes and the planar reflection's own mirror camera. **The Scene view is allowed through**, so you can tune the fog and see it
- **One fog per scene.** It is an atmosphere, not a per-material or per-volume effect. Two components in one scene means the last one enabled is the one driving
- **This is not volumetric fog.** There are no light shafts and no scattering around light sources — it is a distance-and-height haze, which is what a stylised scene wants and what stays viable on Mobile
