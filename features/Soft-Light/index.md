---
layout: docs
title: Soft Light
---

## Soft Light

<div class="compare-container">
  <div class="compare-base">
    <img src="../images/SoftLight-0.jpg" alt="SoftLight-0">
  </div>

  <div class="compare-overlay">
    <img src="../images/SoftLight-1.jpg" alt="SoftLight-1">
  </div>

  <div class="compare-handle"></div>

  <div class="compare-label before">SoftLight : 0</div>
  <div class="compare-label after">SoftLight : 1</div>
</div>

![softlight](../images/softlight.png)

This feature applies a **Soft Light color adjustment (blend mode / tone curve)** to the character’s color **after lighting has been calculated** (Final Lit Color).

It enhances contrast and smoothness for a more subtle, softer look, and is applied **only to the character’s material**, without affecting the scene’s post-processing.

### Parameters

- **Soft Light Highlight** : Controls the strength of the effect by blending between the original color and the Soft Light result

---
