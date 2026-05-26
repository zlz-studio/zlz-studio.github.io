---
layout: docs
title: Dither FX Runtime
last_modified_at: 2026-05-24
---

## Dither FX Runtime

### Demo Dither Occlusion Runtime
![Dither_Occlusion](../Dither/Dither_Occlusion.gif)

### Demo Dither Fade Camera Runtime
![Dither_Fade_Camera](../Dither/Dither_Fade_Camera.gif)

---

### Auto Setup

Done in a single step, just click Setup VFX Features and Refresh Renderers.

![ZLZ_Dither](../Dither/ZLZ_Dither.png)

Adjust Animation Curve

![Dither_Setttings](../Dither/Dither_Setttings.png)

---

### Usage

![upgrade](../images/upgrade.png)

**Upgrade** is a feature used to visualize when a character or weapon is upgraded, providing clear visual feedback to convey progression and change to the player.

### Parameters

Master
- **Enable Dither :** Toggles the whole feature (drives the _DITHER_ON keyword)
- **Pattern - Dither matrix :** Bayer4 (4×4, coarser and sharper) or Bayer8 (8×8, finer and smoother)
- **Animation Settings :** ScriptableObject holding Intro / Outro durations and Occlusion levels

Camera Near Fade
- **Enable :** Activates camera-distance auto-fade
- **Near Distance :** Distance at which the character is fully dithered (default 2 / max 20)
- **Far Distance :** Distance at which the dither begins (default 5 / max 20) Camera ≥ Far → no dither; ≤ Near → full dither; in between → smooth ramp.

> Camera Near Fade works automatically without requiring any script. Simply install the Character Dashboard on the character that needs Dither support.

Receive Occlusion Fade
- **Enable :** Opts this character in to dither out when it blocks the camera-to-target line
- **Level :** Maximum dither preset

> Soft (0.9 = faint silhouette remains)  
> Full (1.0 = fully clipped)  
> Requires a ZLZ_OcclusionFader in the scene with Target Transform pointing at the player — Setup Dither creates one automatically.  

---

### Scripting

Add using ZLZ.AnimeShader; and get a reference to ZLZ_CharacterVFX, then access the Upgrade block:  

```
// Animated (recommended) - plays Intro → Loop → Outro  
vfx.Upgrade.Activate();  
vfx.Upgrade.Deactivate();  
vfx.Upgrade.ToggleUpgrade();  
  
// Check state  
bool active = vfx.Upgrade.IsActive();
``` 

Example - power-up on key press:  

```
void Update()  
{  
    if (Input.GetKeyDown(KeyCode.Q))  
    GetComponent<ZLZ_CharacterVFX>().Upgrade.ToggleUpgrade();  
}
```

Example - buff a player when they pick up a power-up:  

```
void OnPickupPowerUp(GameObject player)  
{  
    player.GetComponent<ZLZ_CharacterVFX>()?.Upgrade.Activate();  
}  
  
void OnPowerUpExpires(GameObject player)  
{  
    player.GetComponent<ZLZ_CharacterVFX>()?.Upgrade.Deactivate();  
}
```
