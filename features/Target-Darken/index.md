---
layout: docs
title: Target Darken FX Runtime
last_modified_at: 2026-05-24
---

## Target-Darken FX Runtine

### Demo Target-Darken Runtime
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

![targetdarken](../images/targetdarken.png)

**Target Darken** is a feature used to reduce the brightness of selected characters in order to draw attention to specific ones, such as characters using skills, appearing in cutscenes, or highlighted during important moments in a scene.

### Concept

The Target Darken system separates control into two levels:

- **Global** : Enables or disables Darken mode for all characters in the scene
- **Local** : Determines whether each individual character is affected by Darken

> Overall concept
> 
> - **Global =** Enables Darken mode for the entire scene
> - **Local =** Selects which characters are *not* darkened

### Parameters

- **TargetDarkenIntensity** **:** Controls how dark the character becomes
    
    > Lower values → the character appears darker
    > 
- **TargetDarkenLocal** **:** Controls whether an individual character is affected by Darken
    - `0` = Not darkened (remains fully lit)
    - `1` = Darkened
- **_TargetDarkenGlobal** : A property intended for developer-side control
    - Not exposed in the Inspector
    - Used to switch all characters into Darken mode simultaneously

### A test script is provided for evaluation

1. Enable the **Target Darken** feature in the character’s material
    - Set `TargetDarkenLocal = 1` → the character can be darkened
    - Set `TargetDarkenLocal = 0` → the character will not be darkened
2. Create an Empty GameObject and attach the script
    
    `Assets/ZLZ_AnimeShader/Demo/Scripts/ZLZ_GlobalDarkenController.cs`

---

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


---
    
3. Test by adjusting the Global Darken value in the script (0 = normal brightness / 1 = darkened)

---
