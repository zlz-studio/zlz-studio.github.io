---
layout: docs
title: v1.1.0 — ZLZ Hub & Lighting
last_modified_at: 2026-09-22
---

[&larr; All versions](/env/changelog/)

# v1.1.0 — ZLZ Hub & Lighting

**Current version**

### New Features

- Added **ZLZ Hub** to check that everything is installed and set up correctly
- Added **Screen Space Reflection** as a second reflection method alongside Planar Reflection — each pixel traces its reflection through the frame, so it works on raised platforms and vertical surfaces without a reflection plane

### Improved Workflow

- Moved lighting and shadows onto a single shared core, used by both **ZLZ Anime Shader** and **ZLZ Env Shader**

### Bug Fixes

- Fixed incorrect namespaces
- Fixed **Planar Reflection** bugs for more accurate reflections
- Fixed **Planar Reflection** not working with **Fog**
- Fixed grass not receiving shadows
- Fixed bugs in baked lighting
- Fixed Renderer Features being added more than once after deleting and reinstalling the package — renderers that already have duplicates are repaired automatically
- Fixed compiler warnings on Unity 6.3 and 6.5

### After Updating

When updating the package, run the folder reorganisation once:

**Window &rsaquo; ZLZ &rsaquo; Hub &rsaquo; Reorganise Folder**
