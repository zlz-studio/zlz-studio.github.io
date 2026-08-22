---
layout: docs
title: Environment Shader Overview
last_modified_at: 2026-08-22
published: false
---

<!-- DRAFT — ยังไม่ขึ้นเว็บจริง. พรีวิว: jekyll serve --unpublished. พร้อมขึ้นเว็บ: ลบ published: false -->
<!-- ฉบับร่างภาษาไทย รอเจ้าของแก้แล้วค่อยแปลงเป็นอังกฤษให้เข้ากับหน้าอื่น -->

# ZLZ Environment Shader

<!-- ![Env_Shader_Overall](../images/Env_Shader_Overall.webp) -->

`ZLZ_Environment_Shader` คือ shader หลักของแพ็กเกจ ใช้กับพื้นผิวทุกอย่างที่คุณปั้นเอง — พื้นดิน หน้าผา ก้อนหิน ต้นไม้ ใบไม้ ตัวอาคาร props — ทุกเมชที่ออกมาจาก Maya หรือ Blender ใช้ตัวนี้ตัวเดียวจบ

ในแพ็กเกจมี shader ทั้งหมด 3 ตัว แบ่งงานกันชัดเจน ไม่ทับกัน:

| Shader | ใช้กับ |
|---|---|
| **ZLZ/Environment/Shader** | พื้นผิวทั่วไปทั้งหมด ← หน้านี้ |
| **ZLZ/Environment/Grass** | ใบหญ้าและดอกไม้ที่ระบบ Grass ปลูกให้ |
| **ZLZ/Environment/Water** | ผิวน้ำ |

- Path : `Assets/ZLZ_EnvironmentShader/Shaders/Core/ZLZ_Environment_Shader.shader`
- Inspector : ใช้ GUI เฉพาะของ ZLZ (`ZLZEnvShaderGUI`) หน้าตาและวิธีใช้เหมือนกับ ZLZ Anime Shader ทุกประการ

---

## ปัญหาที่ shader ตัวนี้แก้

- **ไม่ผูกกับ Unity Terrain** — ระบายเทกซ์เจอร์ทับกันได้ถึง 4 เลเยอร์บนเมชที่คุณปั้นเอง หน้าผาเอียง 70 องศาก็ระบายได้ ถ้ำที่มีเพดานก็ระบายได้ สิ่งที่ Terrain ทำไม่ได้
- **เก็บข้อมูล paint ได้ 2 แบบ** — ลง Mask Texture หรือลง Vertex Color ของเมช เลือกตามไปป์ไลน์ของทีม (Vertex Color = ไม่กินเทกซ์เจอร์เพิ่มเลยสักใบ)
- **Custom Brushes ได้ 4 แบบ** — โดยทั่วไปการ Paint ต้องใช้ Unity Terrain ซึ่งผู้ใช้ Custom Brushes ไม่ได้ แต่ZLZผู้ใช้สามารถ Custom ได้โดยง่าย
- **โทนอนิเมะเป็นค่าตั้งต้น ไม่ใช่ของแถม** — Shadow Color กับ ToonRamp Smoothness เป็นส่วนหนึ่งของแกนหลักที่ปิดไม่ได้ ฉากจึงเข้ากับตัวละคร ZLZ Anime Shader ได้ทันทีโดยไม่ต้องจูน
- **ปิดฟีเจอร์แล้วหายไปจริง ๆ** — ทุกฟีเจอร์ที่ปิดจะถูก strip ออกจาก shader variant ที่คอมไพล์ ไม่ใช่แค่คูณศูนย์ วัสดุที่ใช้แค่ Albedo กับ ToonRamp จึงเบาพอ ๆ กับ unlit shader ธรรมดา
- **ทำงานร่วมกับ URP อย่างที่ environment shader ควรทำ** — Lightmap, Shadowmask, Subtractive, SSAO, URP Decal, Fog ของ Unity, GPU Instancing และ SRP Batcher ใช้ได้ครบโดยไม่ต้องแก้อะไร

---

## หน้าตาของ Inspector

![Features](../shader/Features.png)

เปิดวัสดุขึ้นมาจะเจอ **แผง Features อยู่บนสุด** เป็นตารางปุ่มสำหรับเปิด/ปิดฟีเจอร์ ด้านล่างถัดลงมาคือหมวดค่าต่าง ๆ ที่จะโผล่มาเมื่อฟีเจอร์นั้นถูกเปิด

ฟีเจอร์แบ่งเป็น 2 กลุ่ม

### กลุ่มที่ 1 — Locked (6 หมวด เปิดอยู่ตลอด)

หมวดโครงสร้างหลักที่ปิดไม่ได้ และไม่จำเป็นต้องปิด เพราะแทบไม่มีต้นทุน

| หมวด | ควบคุมอะไร |
|---|---|
| **Rendering** | Render Queue (Opaque / AlphaTest / Transparent), Blend, Cull Mode, ZWrite, ZTest, Alpha Clipping + Cutoff, Cast Shadow |
| **Texture** | Albedo, Tiling / Offset — และถ้าเปิด Triplanar ค่า World Tiling กับ Blend Sharpness จะมาโผล่ในหมวดนี้ |
| **Base Colors** | Base Color, Shadow Color, Texture Brightness |
| **Lighting** | Receive Shadow, Additional Light Intensity |
| **ToonRamp** | Toon Ramp Smoothness — ความคมของขอบระหว่างส่วนสว่างกับส่วนเงา |
| **Transparency** | Alpha Value, Shadow Alpha Clip + Cutoff |

> นอกจาก 6 หมวดนี้ยังมีหมวด **Mask Layout** ซ่อนอยู่ในลิสต์ (ไม่มีปุ่มในแผง Features) ใช้กำหนดว่า Metallic / Smoothness / Emissive ตัวไหนอ่านจากช่องไหนของ Feature Mask — ดูหัวข้อ Feature Mask ด้านล่าง

### กลุ่มที่ 2 — Optional (10 ฟีเจอร์ เปิด/ปิดได้)

| ฟีเจอร์ | ทำอะไร |
|---|---|
| **Paint Mode** | ระบายเทกซ์เจอร์ทับซ้อนได้ถึง 4 เลเยอร์ แต่ละเลเยอร์มี albedo + normal + smoothness + metallic ของตัวเอง |
| **Specular** | ไฮไลต์ Blinn-Phong พร้อมโหมด Toon Highlight ที่ตัดขอบให้เป็นแบบอนิเมะ และมี Metallic ในตัว |
| **Reflection** | เพิ่มชั้นสะท้อนแบบกล้องกระจก (Planar) ทับลงบน probe |
| **Normal Map** | รายละเอียดนูนต่ำบนผิว |
| **Triplanar** | ฉายเทกซ์เจอร์ตามแกนโลก ใช้กับเมชที่ไม่มี UV หรือ UV ยืด |
| **Stochastic** | กำจัดลายซ้ำที่มองเห็นเวลาเทกซ์เจอร์ tile บนพื้นผิวใหญ่ |
| **Accumulation** | หิมะ / ฝุ่น / มอส เกาะบนผิวที่หันขึ้น โดยไม่ต้องวาด mask |
| **Wind** | โยกใบไม้ในขั้น vertex ขับด้วยค่าของวัสดุเองหรือด้วยลมกลางของฉาก |
| **Emission** | ผิวที่เปล่งแสงเอง พร้อมโหมดกะพริบเป็นจังหวะ |
| **Target Darken** | หรี่ทั้งฉากลงเพื่อขับเป้าหมายให้เด่น สั่งจากสคริปต์ตอนรัน |

> ฟีเจอร์ทั้งหมดนี้ตั้งใจให้ตั้งค่าตอนสร้างงาน **ไม่ใช่สลับตอนรันเกม** เพราะการสลับจะสั่ง shader ให้คอมไพล์ variant ใหม่ ยกเว้น Target Darken ที่ออกแบบมาให้ขับจากโค้ดโดยเฉพาะ

---

## Feature Mask — เทกซ์เจอร์ใบเดียว คุม 3 ฟีเจอร์

Metallic, Smoothness และ Emissive ไม่ได้ใช้เทกซ์เจอร์ของใครของมัน แต่แชร์ **Feature Mask (RGBA) ใบเดียวกัน** แล้วให้แต่ละฟีเจอร์เลือกเองว่าจะอ่านจากช่อง R, G, B หรือ A

ผลคือหินเปียกที่มีทั้งความมัน ความเงา และรอยเรืองแสง ใช้เทกซ์เจอร์เพิ่มแค่ **1 ใบ** แทนที่จะเป็น 3 ใบ — ประหยัดทั้ง VRAM และจำนวนครั้งที่ GPU ต้องอ่านเทกซ์เจอร์

และถ้าฟีเจอร์ไหนตั้งช่องเป็น **None** ค่าจากสไลเดอร์จะถูกใช้ทั้งผิวเท่ากันหมด — ถ้าไม่มีฟีเจอร์ไหนใช้ mask เลย GUI จะถอดการอ่านเทกซ์เจอร์ใบนี้ออกจาก shader ให้อัตโนมัติ ไม่เหลือต้นทุนค้างไว้

---

## ZLZ_Env Dashboard — ตัวที่ติดตั้งทุกอย่างให้

วัสดุที่ใช้ shader ตัวนี้ควรอยู่ **ภายใต้ ZLZ_Env Dashboard** เสมอ

เหตุผลคือฟีเจอร์ในฝั่ง Environment ไม่ได้จบที่วัสดุ — Reflection ต้องมี Renderer Feature กับ component บนพื้น, Fog ต้องมีทั้ง Renderer Feature และตัวคุมในฉาก, หญ้าต้องมีกล้องจับสีพื้นกับ renderer ของตัวเอง ถ้าให้ผู้ใช้ไปติดตั้งเองทีละจุดใน URP Asset จะพลาดง่ายมาก และอาการเวลาพลาดคือ "เปิดฟีเจอร์แล้วไม่มีอะไรเกิดขึ้น" ซึ่งหาสาเหตุยากที่สุด

Dashboard แก้ปัญหานี้ด้วยการ **ติดตั้งให้ทั้งหมดตั้งแต่วินาทีที่ใส่ component เข้าไป** และคอยเฝ้าดูว่าชิ้นส่วนไหนหายไปบ้าง

- เพิ่มได้จาก `Add Component > ZLZ/Environment Shader/ZLZ_Env Dashboard`
- หรือคลิกขวาที่ GameObject แล้วเลือก `ZLZ > Setup Env Dashboard`

---

### หมวดต่าง ๆ ใน Dashboard

![Dashboard](../shader/Dashboard.png)

ทุกหมวดมีแถบสถานะบอกว่าชิ้นส่วนครบหรือยัง ถ้าขาดจะมีปุ่มซ่อมให้กดในที่เดียว ไม่ต้องไปตามหาใน URP Asset

| หมวด | ทำอะไรได้ |
|---|---|
| **Vertex Paint Storage** | ดูว่าเมชที่ระบายสีไว้เก็บข้อมูลอยู่ที่ไหน และโยงกับไฟล์โมเดลต้นทางตัวไหน — เวลาแก้โมเดลใน Maya/Blender แล้ว import ใหม่ สีที่ระบายไว้จะตามไปด้วย |
| **Planar Reflection** | สถานะ Renderer Feature + URP_Reflection + component บนพื้น พร้อมปุ่ม `Repair Reflection Setup` |
| **Water** | รายการผืนน้ำทั้งหมด เปิด/ปิดทีละผืน จัดการวัสดุ และเบค Foam Flow |
| **Underwater** | สถานะ Renderer Feature ของภาพใต้น้ำ |
| **Environment Fog** | สถานะ Renderer Feature ของหมอก + ปุ่มสร้าง Fog Global ให้ในฉาก |
| **Grass** | หัวใจของระบบหญ้า — เลือกผิวที่ให้หญ้าขึ้น จัดการ Grass Type ระบายเพิ่มเอง `Grow All` / `Clear All` และตั้งค่าสีพื้น |
| **Grass Performance** | เปิดโหมดวาดหญ้าความละเอียดครึ่ง |
| **Grass Color Capture** | สถานะกล้องจับสีพื้น พร้อมปุ่มซ่อม |
| **VFX Features** | ตั้งค่า Target Darken ของกลุ่มนี้ |

---

### Component ที่ Dashboard ใส่ให้บน object

| Component | ใส่ให้ที่ไหน | เพื่ออะไร |
|---|---|---|
| `ZLZ_EnvPlanarReflectionPlane` | พื้นทุกชิ้นใต้ Dashboard ที่วัสดุเปิด Reflection ไว้ | บอกกล้องกระจกว่าระนาบสะท้อนอยู่ตรงไหน |
| `ZLZ_EnvWater` | ทุกเมชที่ใส่วัสดุน้ำอยู่แล้ว | ค่าที่วัสดุเก็บไม่ได้ เช่น เส้นโค้งจังหวะคลื่น และหมอกใต้น้ำของบ่อนั้น |
| `ZLZ_EnvGrass` | ตัว Dashboard เอง | รากของระบบหญ้า |
| `ZLZ_EnvGrassColorCamera` | ตัว Dashboard เอง | กล้องจับสีพื้นให้หญ้ากลืนกับพื้นด้านล่าง |
| `ZLZ_EnvVFX` | ตัว Dashboard เอง | ตัวรับคำสั่ง Target Darken จากโค้ดเกม |

สอง component สุดท้ายของฝั่งหญ้าจะ **ซ่อนอยู่จนกว่าจะเริ่มปลูกหญ้าจริง** เพื่อไม่ให้เห็นช่องติ๊กของระบบที่ยังไม่ได้ใช้แล้วเผลอเปิดครึ่ง ๆ กลาง ๆ

---

### จัดฉากเป็น 3 Dashboard : Terrain / Env / Water

![Prefab](../shader/Prefab.png)

วิธีที่แนะนำคือแบ่งของในฉากออกเป็น 3 กลุ่ม กลุ่มละ Dashboard (ดูตัวอย่างได้จาก `Demo/Prefab/Base_Terrain`, `Base_Env`, `Base_Water`)

| Dashboard | เก็บอะไรไว้ข้างใน | ใช้ shader ตัวไหน |
|---|---|---|
| **Terrain** | พื้นดิน หน้าผา ภูมิประเทศ — ผิวที่ปลูกหญ้า ระบายเทกซ์เจอร์ และสะท้อนตอนเปียก | **ZLZ_Environment_Shader** + ZLZ_Environment_Grass |
| **Env** | props ทั้งหมด — ต้นไม้ ใบไม้ ก้อนหิน อาคาร เห็ด ของตกแต่ง | **ZLZ_Environment_Shader** |
| **Water** | ผิวน้ำทุกผืนในฉาก | ZLZ_Environment_Water |

`ZLZ_Environment_Shader` จึงอยู่ได้ทั้งใต้ Terrain และ Env — ขึ้นอยู่กับว่าชิ้นนั้นทำหน้าที่อะไร ไม่ใช่ว่าใช้ shader อะไร

การแยกแบบนี้ไม่ใช่แค่ความเป็นระเบียบ แต่มีผลจริง เพราะ Dashboard ทำงานกับ **ทุกเมชที่อยู่ใต้ตัวมัน** — Terrain Dashboard จึงเห็นเฉพาะพื้นที่ควรปลูกหญ้า ส่วน Water Dashboard เห็นเฉพาะผืนน้ำ ไม่ปนกัน

---

### Renderer Features ที่ Dashboard ติดตั้งให้

![RenderFeatures](../shader/RenderFeatures.png)

พอเพิ่ม Dashboard เข้าไป มันจะใส่ Renderer Feature 4 ตัวนี้ให้ทันที และใส่ให้ **ทุก Quality Level** ไม่ใช่แค่ tier ที่เปิดอยู่ตอนนั้น

| Renderer Feature | ติดตั้งเพื่อฟีเจอร์อะไร |
|---|---|
| **ZLZ Env Planar Reflection** | ชั้นสะท้อนแบบกล้องกระจก — ใช้ร่วมกันทั้งพื้นเปียกและผิวน้ำ |
| **ZLZ Env Fog** | หมอกของฉาก : หมอกระยะไกล หมอกพื้น และสีท้องฟ้าที่เข้ากัน |
| **ZLZ Env Grass Resolution** | โหมดประหยัดของหญ้า — วาดหญ้าที่ความละเอียดต่ำกว่าภาพหลักแล้วผสมกลับ |
| **ZLZ Env Underwater** | ภาพตอนกล้องอยู่ใต้น้ำ : หมอกใต้น้ำ และการเปลี่ยนผ่านตอนดำลงไป |

นอกจากนี้ยังสร้าง **renderer เพิ่มอีก 2 ตัว** ซึ่งไม่ได้มากับแพ็กเกจแต่ generate ขึ้นในโปรเจกต์คุณ:

- **URP_Reflection** — renderer เฉพาะของกล้องกระจก (ตั้งหญ้าไว้ที่ Quarter) เพื่อไม่ให้กล้องสะท้อนวนเรียกตัวเอง และให้ภาพสะท้อนไม่กิน performance เท่าภาพหลัก
- **Grass Colour Capture Renderer** — renderer เปล่าสำหรับกล้องมองลงที่จับสีพื้นให้หญ้า

> **ติดตั้งตั้งแต่ตอน Import** — งานชุดนี้ทำงานตอน import แพ็กเกจด้วย ไม่ต้องรอให้ใส่ Dashboard ก่อน เปิด demo scene มาแล้วเห็นภาพถูกต้องเลย และเวลาอัปเดตเวอร์ชัน โปรเจกต์เดิมจะได้ Renderer Feature ตัวใหม่ที่เพิ่งเพิ่มมาโดยอัตโนมัติ ทุกขั้นตอนเป็น idempotent คือรันซ้ำกี่รอบก็ไม่เกิดของซ้อน

---

### Script ที่ Dashboard ไม่ได้ดูแล

ของสองกลุ่มนี้อยู่นอก Dashboard เพราะขอบเขตงานไม่เหมือนกัน

**ระดับ object — ติดบนตัวที่เคลื่อนไหว ไม่ใช่บนฉาก**

| Component | ติดที่ไหน |
|---|---|
| `ZLZ_EnvGrassInteractor` | ตัวละครหรือวัตถุที่ต้องการให้หญ้าแหวกตอนเดินผ่าน |
| `ZLZ_EnvWaterInteractor` | ตัวละครหรือวัตถุที่ต้องการให้เกิดระลอกคลื่นตอนแตะน้ำ |
| `ZLZ_EnvWaterFloater` | วัตถุที่ต้องลอยขึ้นลงตามผิวน้ำ เช่น เรือ ทุ่น ท่อนไม้ |

**ระดับฉาก — มีตัวเดียวต่อฉาก ใช้ร่วมกันทุก Dashboard**

| Component | ทำอะไร |
|---|---|
| `ZLZ_EnvWindController` | ลมกลางที่ทั้งต้นไม้ ใบไม้ และหญ้าโยกตามพร้อมกัน |
| `ZLZ_EnvGrassController` | ระยะ LOD และ Quality Preset ของหญ้าทั้งฉาก |
| `ZLZ_EnvFog` | ค่าหมอกของฉาก |
| `ZLZ_EnvDarkenManager` | ตัวสั่งหรี่ฉากจากโค้ดเกม พร้อมไฟล์ตั้งค่าจังหวะการหรี่ |

สามตัวแรกสร้างพร้อมกันได้ในคลิกเดียวจาก `GameObject > ZLZ > Setup ZLZ Global` ซึ่งจะได้ GameObject ชื่อ `ZLZ_Global` มาเป็นที่รวม

> Dashboard **ไม่สร้าง Global ให้เอง** โดยตั้งใจ เพราะการที่แค่เปิดฉากขึ้นมาดูแล้วมี GameObject ใหม่โผล่ในฉากจะทำให้ทุกฉากที่คุณเปิดกลายเป็น dirty ต้องกด save โดยไม่ได้แก้อะไรเลย

---

### ตอนรันเกม Dashboard ไม่มีต้นทุน

ตัว Dashboard **ไม่เก็บข้อมูลของตัวเองเลย และไม่มีโค้ดที่ทำงานตอนรัน** — ข้อมูลจริงอยู่บนวัสดุ, บนไฟล์ `ZLZ_EnvGrassData` และใน registry asset ส่วน UI ทั้งหมดเป็นโค้ดฝั่ง Editor ที่ถูกตัดออกตอน build

ผลที่ตามมามี 2 อย่าง:

- ในเกมจริงมันเป็นแค่ marker เปล่า ๆ ไม่กิน CPU สักนิด และแพลตฟอร์มที่ strip script ที่ไม่ใช้ทิ้งก็ไม่เสียอะไร
- ระบบอัตโนมัติทำงานได้แม้ไม่ได้เปิดฉากไหนอยู่ และใช้ได้ทั้งกับ Dashboard ที่อยู่ในฉากและที่อยู่ใน Prefab

---
