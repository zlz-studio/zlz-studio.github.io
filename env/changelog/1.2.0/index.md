---
layout: docs
title: v1.2.0 — Hotfix › Install
last_modified_at: 2026-10-20
---

[&larr; All versions](/env/changelog/)

# v1.2.0 — Hotfix &rsaquo; Install

### Bug Fixes

- Fixed compile error **CS1069** in projects with the built-in **VR** module disabled, such as new projects from Unity 6.3+ templates. Fog, Underwater and Planar Reflection no longer use it, so the package no longer depends on that module
