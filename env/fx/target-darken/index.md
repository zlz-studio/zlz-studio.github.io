---
layout: docs
title: Target Darken FX Runtime
last_modified_at: 2026-08-02
published: false
---

<!-- DRAFT — ยังไม่ขึ้นเว็บจริง. พรีวิว: jekyll serve --unpublished. พร้อมขึ้นเว็บ: ลบ published: false -->

# Target Darken FX Runtime

<!-- TODO (step 1) : เขียนเนื้อหาภาษาไทย โดยอ่านจาก source จริง
     - Shaders/Features/ZLZ_EnvDarken.hlsl
     - Runtime/ZLZ_EnvDarkenManager.cs  (Darken / Restore / ToggleDarken / SetInstant / IsActive / Instance)
     - Runtime/ZLZ_EnvVFX.cs            (Darken.Exclude / Include / SetExcluded / IsExcluded)
     - Runtime/ZLZ_EnvDarkenSettings.cs

     โครงหน้าให้ตามแบบ FX Runtime ของ ZLZ Anime Shader (features/Target-Darken/)
     ไม่ใช่แบบหน้า Environment Shader ทั่วไป :
       Demo -> Auto Setup -> Concept (Global / Local) -> Parameters -> Scripting (โค้ด C#)

     ประเด็นที่ต้องไม่ลืม :
       - Global x Local สองชั้น (Global เปิดโหมดทั้งฉาก / Local เลือกว่าใครไม่โดน)
       - Env กับ Anime Shader เขียนลง _TargetDarkenGlobal ตัวเดียวกันโดยตั้งใจ
         ตัวละครกับพื้นจึงมืดพร้อมกัน แต่ถ้ามี ZLZ_DarkenManager ของ Anime อยู่ในฉาก
         ตัวของ Env จะถอยให้ (IsDeferred)
       - วัสดุตัวละครถือ Darken Intensity ของตัวเอง ถ้าใช้ร่วมฉากต้องตั้งให้เท่ากัน
-->
