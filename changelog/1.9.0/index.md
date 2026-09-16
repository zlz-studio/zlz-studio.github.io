---
layout: docs
title: v1.9.0 — ZLZ Hub & Lighting
last_modified_at: 2026-09-16
---

[&larr; All versions](/changelog/)

# v1.9.0 — ZLZ Hub & Lighting

**Current version**

> **Known critical issue.** The Lighting folder was left out of this package export, so v1.9.0 cannot be used after updating. The fix is in **[v2.0.0](/changelog/2.0.0/)**, which is currently in Unity Asset Store review — please hold off on updating to v1.9.0 until it is published.

### New Features

- Added **ZLZ Hub** to verify the package is installed correctly and completely
- Added **Probe Occlusion** support so characters receive baked lighting correctly
- Added **per-texture UV channel selection**

### Improved Workflow

- Moved lighting and shadows onto a single shared core, used by both **ZLZ Anime Shader** and **ZLZ Environment Shader**

### Bug Fixes

- Fixed **Additional Light** not working on Forward+
- Fixed features that depend on a Directional Light breaking when it is turned off — they now keep working from a fallback light direction
