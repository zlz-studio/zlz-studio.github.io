## Target Darken

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
    
3. Test by adjusting the Global Darken value in the script (0 = normal brightness / 1 = darkened)

---
