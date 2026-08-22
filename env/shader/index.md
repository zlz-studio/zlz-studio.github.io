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

## Material Presets

แพ็กเกจมีวัสดุตัวอย่างพร้อมใช้ใน `Assets/ZLZ_EnvironmentShader/Material_Preset/` สำหรับดูว่าค่าที่จูนมาแล้วหน้าตาเป็นยังไง:

- **ZLZ_Env_Triplanar** — หน้าผา / บล็อกเอาต์ ที่ฉายเทกซ์เจอร์ตามแกนโลก
- **ZLZ_Env_Leave_Netural01 / 02** — ใบไม้โทนธรรมชาติ
- **ZLZ_Env_Leave_Stylized01 / 02** — ใบไม้โทนอนิเมะจัด
- **ZLZ_Env_All_Reflection** — ผิวสะท้อนที่เปิดทั้ง probe และ planar

---
