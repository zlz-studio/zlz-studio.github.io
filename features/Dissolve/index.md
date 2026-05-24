---
layout: docs
title: Dissolve FX Runtime
last_modified_at: 2026-05-24
---

## Dissolve FX Runtine

### Demo Dissolve Runtime
![Demo_GetHit](../GetHit/Demo_GetHit.gif)

---

### Auto Setup

Done in a single step, just click Setup VFX Features and Refresh Renderers.

![VFX_Features](../Upgrade/VFX_Features.png)

![Character_VFX](../Upgrade/Character_VFX.png)

Adjust Animation Curve

![GetHit_Settings](../GetHit/GetHit_Settings.png)

---

### Usage

![dissolve](../images/dissolve.png)

Dissolve Character is used to gradually fade a character out of the scene. It is commonly applied when a character dies, warps, or is removed from the scene. The effect allows flexible control over the pattern, color, and timing of the dissolve.

### Parameters

- **Texture2DDissolve :** Uses a noise texture to define the dissolve pattern. *Tiling* can be adjusted to achieve the desired look
- **Dissolve Color :** Adjusts the color of the glow displayed along the dissolve edges
- **Dissolve Value :** Controls the dissolve state *(0 = disabled / 1 = fully dissolved and invisible)*
- **Start Dissolve :** Adjusts the starting point of the dissolve effect. *Lower this value if the effect starts too quickly or appears abrupt; increase it if the effect starts too late*
- **End Dissolve :** Adjusts the ending point of the dissolve effect. *Used to fix cases where the character is still partially visible when Dissolve Value reaches 1, or disappears too quickly*
- **SizeGlowDissolve :** Controls the size of the glow edge displayed during the dissolve

### Scripting

Add using ZLZ.AnimeShader; and get a reference to ZLZ_CharacterVFX, then access the GetHit block:  

> // Trigger the hit flash - plays Intro → Loop → Outro, auto-fades  
> vfx.GetHit.Hit();  
> vfx.GetHit.Deactivate();   // cancel mid-flash (rare)  
>   
> // Check state  
> bool active = vfx.GetHit.IsActive();  
  
Example - flash on taking damage:  
  
> void TakeDamage(int amount)  
> {  
>     health -= amount;  
>     GetComponent<ZLZ_CharacterVFX>().GetHit.Hit();  
> }  
  
Example - flash an enemy when hit by a player attack:  
  
> void DealDamage(GameObject enemy, int amount)  
> {  
>     enemy.GetComponent<ZLZ_CharacterVFX>()?.GetHit.Hit();  
> }  
