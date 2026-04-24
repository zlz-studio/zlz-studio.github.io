---
layout: docs
title: Outline
---

## Outline

<div class="compare-container">
  <div class="compare-base">
    <img src="../images/Outline_Off.jpg" alt="Outline_Off">
  </div>

  <div class="compare-overlay">
    <img src="../images/Outline_On.jpg" alt="Outline_On">
  </div>

  <div class="compare-handle"></div>

  <div class="compare-label before">Outline Off</div>
  <div class="compare-label after">Outline On</div>
</div>

<div class="compare-container">
  <div class="compare-base">
    <img src="../images/Outline_Example1.jpg" alt="Outline_Example1">
  </div>

  <div class="compare-overlay">
    <img src="../images/Outline_Example2.jpg" alt="Outline_Example2">
  </div>

  <div class="compare-handle"></div>

  <div class="compare-label before">Outline Showcase Color Example1</div>
  <div class="compare-label after">Outline Showcase Color Example2</div>
</div>

<div class="compare-container">
  <div class="compare-base">
    <img src="../Outline/Outline_Z_Offset_0.jpg" alt="Outline_Z_Offset_0">
  </div>

  <div class="compare-overlay">
    <img src="../Outline/Outline_Z_Offset_0.025.jpg" alt="Outline_Z_Offset_0.025">
  </div>

  <div class="compare-handle"></div>

  <div class="compare-label before">Outline Z Offset : 0</div>
  <div class="compare-label after">Outline Z Offset : 0.025</div>
</div>

![outline](../images/outline.png)

### Parameters

- **Outline Z Mode :** Provides two modes:
    - **Legacy —** Allows the use of Outline Z Offset, but may cause issues when used with certain types of reflections
    - **Planar Safe —** Resolves compatibility issues with some reflection types, but disables the use of Outline Z Offset
- **Outline Width :** Adjusts the thickness of the outline
- **Outline Intensity :** Controls the brightness of the outline *(0 = black / 1 = uses the Base Color from the Main Texture)*
- **Outline Color :** Directly sets the outline color
- **Outline Z Offset :** Adjusts the offset distance of the outline from the character surface, used to prevent z-fighting with the surface or to increase outline prominence

---
