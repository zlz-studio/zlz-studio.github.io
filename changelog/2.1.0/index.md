---
layout: docs
title: v2.1.0 — Hotfix › Lighting & Setup
last_modified_at: 2026-10-20
---

[&larr; All versions](/changelog/)

# v2.1.0 — Hotfix &rsaquo; Lighting & Setup

### Bug Fixes

- Fixed shadows not following the **Environment Lighting Intensity Multiplier**. Ambient light now reaches the lit and shadow sides equally, and the lit side is tinted by Base Color the way the shadow side is tinted by Shadow Color
- Fixed duplicate **Renderer Features** after deleting and re-importing the package. Setup now keeps one of each ZLZ feature and repairs renderers that already have duplicates
- Fixed setup switching off **Contact Shadow**, **Screen Space Outline** and **Selection Outline** when run again. Installed features now keep your settings
- Fixed the material inspector compile error on Unity 6000.0 and the obsolete API warning on Unity 6.3 and newer
- Fixed Unity 6.5 deprecation warnings (FindObjectsByType)

### Changed

- Character ambient lighting is now uniform across the body. Characters with a non-white Base Color look slightly different from v2.0.0

### Removed

- The unused file `Shaders/Lighting/ZLZ_MainLighting.hlsl`. Updating does not delete it; you can remove it yourself
