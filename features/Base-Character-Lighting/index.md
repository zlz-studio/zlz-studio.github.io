---
layout: docs
title: Base Character Lighting
last_modified_at: 2026-04-23
---

# Base Character Lighting

<div class="compare-container">
  <div class="compare-base">
    <img src="../images/ReceiveShadow_Off.jpg" alt="ReceiveShadow_Off">
  </div>

  <div class="compare-overlay">
    <img src="../images/ReceiveShadow_On.jpg" alt="ReceiveShadow_On">
  </div>

  <div class="compare-handle"></div>

  <div class="compare-label before">Realtime Lighting ReceiveShadow Off</div>
  <div class="compare-label after">Realtime Lighting ReceiveShadow On</div>
</div>

<div class="compare-container">
  <div class="compare-base">
    <img src="../images/ShadowMask_PointLight_Off.jpg" alt="ShadowMask_PointLight_Off">
  </div>

  <div class="compare-overlay">
    <img src="../images/ShadowMask_PointLight_On.jpg" alt="ShadowMask_PointLight_On">
  </div>

  <div class="compare-handle"></div>

  <div class="compare-label before">Baked ShadowMask Point Light Off</div>
  <div class="compare-label after">Baked ShadowMask Point Light On</div>
</div>

![basecharacterlighting](../images/basecharacterlighting.png)

This section controls the character’s lighting and shadow behavior.

### Parameters

- **Receive Shadow** : Enables or disables receiving shadows cast by other objects *(disabling this option does **not** affect the character’s ability to cast shadows onto surrounding objects)*
- **Additional Light Intensity** : Controls the intensity of secondary lights such as Point Lights and Spot Lights; this value does not affect the main Directional Light in the scene

last_modified_at: 2026-04-23
---
