---
layout: docs
title: Grass Overview
last_modified_at: 2026-08-23
published: false
---

<!-- DRAFT — ยังไม่ขึ้นเว็บจริง. พรีวิว: jekyll serve --unpublished. พร้อมขึ้นเว็บ: ลบ published: false -->

# ZLZ Grass System

<!-- ![Grass_Overall](../images/Grass_Overall.webp) -->

The ZLZ grass system is not a single shader — it is a **complete system for planting grass onto meshes you made yourself**, covering the scatter, the storage, the LOD, the wind, and the way blades part as a character walks through them.

| Piece | What it is |
|---|---|
| **ZLZ/Environment/Grass** | The shader that draws the blades and flowers |
| `ZLZ_EnvGrass` | Plants the field and draws it. Lives on the Dashboard |
| `ZLZ_EnvGrassType` | A preset asset : "this is what this kind of grass looks like" |
| `ZLZ_EnvGrassData` | The file holding the tufts you have grown |
| `ZLZ_EnvGrassController` | LOD for all the grass in the scene |

- Path : `Assets/ZLZ_EnvironmentShader/Shaders/Core/ZLZ_Environment_Grass.shader`
- Everything is driven from **ZLZ_Env Dashboard > Grass** — no hunting for components one at a time

---

## What This System Solves

- **Not tied to Unity Terrain** — plant straight onto meshes sculpted in Maya or Blender. A sloping hillside, an island in the middle of a lake, or ground assembled from several pieces all work
- **Planted in one click** — pick the Mask Texture Channel, pick the grass type, press `Grow All`. No placing tufts by hand
- **The data does not live in the scene** — every tuft is written into a `ZLZ_EnvGrassData` file, the same pattern Unity Terrain uses with TerrainData. Scene files stay small, and a grown field travels with its Prefab
- **Nothing is baked out as a mesh** — the system stores only the *position* of each tuft and draws them with GPU Instancing from one shared blade mesh. No huge mesh assets appearing in your project
- **Several types at once** — grass and flowers share one field, each with its own density, size and meshes
- **Clean edges with no hand-tweaking** — blades shorten themselves along the boundary with a path, keep clear of props, and hold a settable distance in from the mesh edge
- **Colour matches the ground automatically** — grass samples the colour of the surface it grows on, so it never reads as a foreign green sheet laid over dirt, sand or stone
- **Sways to the same wind as the trees** — the same `ZLZ_Env Wind Controller` the leaves use, so the whole scene moves as one
- **Parts as a character walks through** — with no per-blade colliders
- **LOD and a performance mode built in** — thinner far out, an optimized mesh beyond that, and a limit past which it stops drawing, plus a mode that draws grass below full screen resolution
- **Runs on PC and Mobile** — swap the entire LOD set with Unity's Quality Level, with nothing re-grown

---

## How Grass Comes to Exist

{% include youtube-loop.html id="jG3f89pkjh4" %}

Every step lives in the **Grass** section of the Dashboard that owns that ground (normally the Terrain Dashboard).

1. **Choose which surfaces grow grass** — the Dashboard lists every mesh underneath it with a tick box. Tick only the ones you want. If a mesh carries several materials, a second row of tick boxes lets you choose which material grows grass (grass on the grass material, none on the rock material)
2. **Choose a Grass Type** — pick one of the presets that ship with the package, or press `Create New` for your own. Several types can run at once
3. **Press `Grow All`** — the system scatters tufts across the whole surface at each type's density, then writes the result into the Grass Data file

If you want tighter control than that, a **hand-painting mode** lets you add or remove tufts spot by spot in the Scene view.

> **Most values apply live**, with nothing to re-grow — height, edge distances, edge softness and the mask threshold can all be dragged in real time. Only the values that decide *where a tuft lands* (density, clustering, blocking layers) need a fresh `Grow`.

---

## How the Grass Material Is Laid Out

![Grass_Features](../grass/grass-material/Features_Properties.png)

The layout matches `ZLZ_Environment_Shader` exactly — the **Features panel at the very top**, with the value sections below it.

### Group 1 — Locked (4 sections, always on)

| Section | What it controls |
|---|---|
| **Rendering** | Alpha Cutoff (the threshold that cuts out the blade shape) and Cast Shadow |
| **Texture** | The blade texture — RGB for colour, Alpha for the blade's shape |
| **Colors** | Base Color, Shadow Color, and the Height Gradient running from base to tip |
| **Lighting** | Receive Shadow, Additional Light Intensity |

### Group 2 — Optional (4 features you switch on and off)

| Feature | What it does |
|---|---|
| **Wind** | Vertex-stage sway, taking the wind from the material itself or from the scene wind, with Leaf Flutter and Small Blade — which makes short blades move less than tall ones |
| **Wind Gust Wave** | A band of light and shade sweeping across the field along the wind, so a large lawn reads as gusts rolling over it rather than a still green carpet |
| **Ground Color** | Samples the colour of the ground the grass stands on, blending it in at the base and fading out toward the tip |
| **Interaction** | Blades lean away from anything that comes near |

> There is also a **Debug** section with no button in the Features panel. It shows one stage at a time — the wind mask, the distance fade, the main light, the interaction radius and the gust wave — so you can check each is behaving.

---

## Grass Type — One Preset per Kind of Grass

![Grass_Type](../grass/Grass_Type.png)

Instead of wiring a Material to a Mesh in every place they are used, everything that describes "what this kind of grass looks like" is stored in a single asset file, reusable across every mesh and every scene.

| Group | What is in it |
|---|---|
| **Shape** | Material, the tuft meshes (one picked at random per tuft), and an optimized mesh for the far field |
| **Amount** | Density in tufts per square metre, and a cap on the tuft count so one type can never explode across a huge surface |
| **Size** | A random size range, and a Height Offset that sinks the base slightly into the ground so blades do not float |
| **Clustering** | How tightly this type clumps, and how wide each clump is |
| **Edges** | Distance kept from painted ground, distance kept in from the mesh edge, and the band over which blades taper down instead of ending on a hard line |

Clustering is what makes flowers read as flowers. Grass scattered evenly looks right; flowers scattered evenly look like spilled salt. In nature flowers spread in patches, so leave this at 0 for grass and raise it for flowers.

---

## How the System Decides Where Grass Grows

![Grass_Debug](../grass/Grass_Debug.png)

Four layers of control stack on top of each other.

**1. The base mode** — grass either covers the whole surface, or **the ground's own Paint system decides**. Choose the latter and the grass reads the Mask Texture or the Vertex Colors, according to what the ground material itself is set to — there is no second place to configure it, so ground and grass always agree.

**2. The mask threshold** — draggable live. The higher it goes, the more grass restricts itself to where the mask is genuinely strong. Useful for clearing a path.

**3. Blocking Layers** — pick which layers block grass. Only colliders on those layers make a bald patch, which means **the props in your level can keep grass out while your characters walk straight through it**.

**4. Edge distances** — distance from painted ground, from the mesh edge, and from blocking objects. All three apply live, and Shrink Near Edges tapers blades down before they end rather than cutting them on a hard line.

---

## Where a Blade's Colour Comes From

![BaseColors_Properties](../grass/grass-material/BaseColors_Properties.png)

The colour you see on a single blade does not come from one field. Four layers stack up.

| Layer | Where it comes from | Which section |
|---|---|---|
| **1. Base colour** | The blade texture (RGB) multiplied by Base Color | Texture + Colors |
| **2. Colour in shadow** | Shadow Color — the colour a blade becomes in shadow, rather than simply going darker | Colors |
| **3. Gradient up the blade** | Height Gradient, running from Gradient Bottom at the base to Gradient Top at the tip, with Gradient Power setting how high the changeover sits | Colors |
| **4. Colour from the ground below** | Ground Color — samples the surface the grass stands on | Ground Color (toggleable) |

> **The first three are enough for most grass.** Height Gradient already gives you a deep base fading to a light tip, so plenty of projects use nothing but a blade mesh with an alpha cutout and no colour texture at all.

---

### Ground Color — Letting Grass Take Colour From the Ground

![Camera_Grass](../grass/Camera_Grass.png)

The colour map can come from one of two places, chosen in the **Ground Color** dropdown under `Dashboard > Grass`.

| Mode | Where the colour map comes from | Best for |
|---|---|---|
| **Ortho Camera** | An orthographic camera looking straight down, capturing the ground **after it has been lit**. The grass uses that colour directly with no second lighting pass — so a shadow moving across the ground moves across the grass with it | Scenes where the light changes, shadows move, or the ground is edited often while you work |
| **Baked** | A texture baked ahead of time and assigned by hand in the Baked Map slot. No extra camera in the scene at all | Scenes with static lighting, where the runtime cost has to be zero |

---

### Where to Adjust the Ortho Camera

![Grass_Color_Camera](../grass/ZLZ_Grass_Color_Camera.png)

Choose **Ortho Camera** and the system adds the `ZLZ_EnvGrassColorCamera` component for you, on the **same GameObject as the Dashboard**, already enabled. There is no camera to set up by hand.

You can adjust it in two places — at **`Dashboard > Grass Color Capture`** (which also carries a status strip and a repair button), or on the component directly.

| Setting | What it does |
|---|---|
| **Capture Mask** | The layers the camera captures. Leave it at `Nothing` and the system uses the layers of every surface currently growing grass |
| **Resolution** | Resolution of the colour map (64–1024, default 256) — it is a broad colour tone, so low is plenty |
| **Renderer Index** | Which renderer in the URP Asset the camera renders through |
| **Update Mode** | `Once` captures at start then disables the camera / `Every Seconds` re-captures on a timer / `Every Frame` re-captures every frame |
| **Re-capture triggers** | Capture again when a surface moves, and when the main light changes direction, colour, intensity or temperature |
| **Capture Now** | Take one capture right away |

> **Renderer Index matters more than it looks.** The camera has to render through a bare renderer carrying no Renderer Features — otherwise the captured colour is processed a second time by Tone Mapping / Outline / SSAO, and the grass stops matching the ground. The Dashboard creates a renderer named `URP_Grass` and points the camera at it for you the moment you choose the mode.

> **Only one colour camera may run per scene**, because the colour map is a single global — two of them would overwrite each other every frame. The Dashboard disables the others for you once you choose which one owns the capture.

> **At runtime the camera barely works at all.** `Once` captures and then switches itself off; the other modes only enable it on the frames a capture is actually due, and everything in between is a cheap has-anything-changed check. While you are editing it refreshes on its own as you change the ground, so you see the result without pressing anything.

---

## Scene Level — ZLZ_Env Grass Controller

![Grass_LOD_Settings](../grass/Grass_LOD_Settings.png)

LOD distances do not live on each patch of grass. They live on **one central controller for the whole scene**, which pushes its values out to every patch — so no field ever ends up with distances out of step with its neighbours.

Create it in one click from `GameObject > ZLZ > Setup ZLZ Global`, which brings the wind and fog controllers along in the same place.

What it controls:

- **The distance where grass starts thinning out**, and what fraction of it survives out there
- **The distance where it stops drawing**
- **The fade band** — grass dissolves gradually through a dither before the cut-off, so you never see a hard-edged circle travelling with the camera
- **The band where it stops receiving shadows** — distant grass skips the shadow-map read entirely, a large saving on a dense field

### Quality Preset — Swap the Whole Set With the Quality Level

The values above can be saved into a **Quality Preset** file, and each file mapped to one of Unity's Quality Levels.

Six ship with the package : `PC_Low / PC_Mid / PC_High` and `Mobile_Low / Mobile_Mid / Mobile_High`.

So a game shipping several quality tiers **swaps its entire grass LOD just by changing the Quality Level, with not one tuft re-grown** — and because they are assets, the same preset serves every scene in the project.

---

## Performance

| Technique | What it buys |
|---|---|
| **GPU Instancing** | One blade mesh reused for every tuft, submitted in batches |
| **Cell-based culling** | The per-frame work loops over *cells* (a few hundred) rather than *tufts* (hundreds of thousands). Each cell is frustum- and distance-tested on the CPU, then the whole cell goes out in a single draw |
| **Grass Resolution** | Draws the grass colour into a buffer smaller than the screen and composites it back — the overdraw is paid at a fraction of the pixels, while shadows stay at full resolution |
| **Mesh Baker** | Builds a mesh from a Texture2D sample, so you never have to make one in Maya or Blender |
| **Shadows dropped at distance** | Skips the shadow-map read, which is heavy work once multiplied by the pixel count of a whole field |

> **Grass deliberately does not receive SSAO.** A dense field of thin alpha-tested blades reads as a blotchy smear under SSAO, and the field also casts its AO down onto the ground beneath it, mottling the lawn. Blades keep their clean toon shading instead — the same shading the character shader uses.

---
