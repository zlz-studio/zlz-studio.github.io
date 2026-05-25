---
layout: docs
title: Features Overview
last_modified_at: 2026-04-25
---

# Features

![Features_Overall](./images/Features_Overall.png)

The features of this shader are designed to be divided into **two main groups**, based on their roles and impact on performance.

### 1. Locked Features

These features form the core systems of the shader and are designed to be **always enabled**.

- They provide the fundamental structure required for the shader to function
- They cannot be disabled
- Enabling these features does not impact performance

### 2. On / Off Features

These features are optional, allowing users to enable or disable them as needed.

- They can be toggled on or off through the material
- When disabled, the system removes the related calculations entirely, helping maintain appropriate shader performance
- Features in this group cannot be toggled at runtime and should be configured during setup or before use

---

## Texture Character

![TextureCharacter](./images/TextureCharacter.png)

This shader uses **two main types of textures**, as described below:

### Main Texture (RGBA)

The primary texture used for rendering the character’s surface.

### Parameters

- **RGB (Red, Green, Blue) :** Defines the character’s base color
- **A (Alpha) :** Controls the character’s transparency; darker masked areas become more transparent

---

## Mask Layout (Update in v 1.5.0)

![Mark_LayOut](./images/Mark_LayOut.png)

A mask texture management system for ZLZ Anime Shader, packs masks for multiple features into a single texture to save VRAM and reduce texture samples

### Feature Mask (RGBA)

A texture used to control the operating areas of various features by masking each channel.

### Concept

- One texture has 4 channels (R/G/B/A) → 1 channel = 1 feature mask → 1 texture supports 4 features.
- ZLZ Anime Shader supports up to 2 mask textures = 8 features total.

### Default Layout:
- Mask 1: R = Metallic, G = Hair Highlight, B = Emissive, A = Outline
- Mask 2: R = Specular

### How to Use

Each feature has a "Use Mask" checkbox:
- ✓ Checked : Reads mask from the selected channel → feature applies according to the mask
- ☐ Unchecked : 	Feature applies to the whole mesh → no texture needed

> You can change each feature's dropdown to point to a different channel if you want to design your own layout,  
> if any feature crosses into Mask 2, the Mask 2 slot appears automatically.  

