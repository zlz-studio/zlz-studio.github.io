---
layout: docs
title: v1.2.0 — Face Shadow Accuracy & Workflow Update
last_modified_at: 2026-07-11
---

[&larr; All versions](/changelog/)

# v1.2.0 — Face Shadow Accuracy & Workflow Update

This version focuses on improving the Face Shadow system and overall workflow, making it more accurate, easier to use, and visually refined. It also provides better control over behavior in the Editor, reducing unnecessary material changes and preventing unwanted Version Control updates.

### What's New

- Improved facial lighting and shadow behavior to correctly respond to light direction
- Enhanced accuracy of nose and facial shadow rendering
- Smoother shadow fading, reducing harsh edges and abrupt cutoffs
- Updated Face Shadow system to be driven by the **Head Bone**, improving compatibility with non-standard character orientations and overall accuracy
- Added a helper to notify users if the required script or Head Bone for Face Shadow is not assigned
- Updated Script: **ZLZ_Head Direction Binder** to support enabling/disabling Editor Preview:
  - **Enabled:** the script runs in the Editor, ensuring correct visual results immediately, but updates material values, which may cause changes in Version Control
  - **Disabled:** the script runs only at Runtime and does not send values to materials in the Editor, preventing changes in Version Control
