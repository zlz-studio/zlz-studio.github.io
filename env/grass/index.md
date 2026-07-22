---
layout: docs
title: Grass Overview
last_modified_at: 2026-07-15
published: false
---

<!-- DRAFT — ยังไม่ขึ้นเว็บจริง. พรีวิว: jekyll serve --unpublished. พร้อมขึ้นเว็บ: ลบ published: false -->

# Grass

## ShowcasePaintMode - Control
{% include video.html src="/env/paint-mode/PaintMode_Controller_Web.mp4" %}

## ShowcasePaintMode - Texture
{% include video.html src="/env/paint-mode/PaintMode_Texture_Web.mp4" %}

## ShowcasePaintMode - Brush
{% include video.html src="/env/paint-mode/PaintMode_Brush_Web.mp4" %}

## ShowcasePaintMode - Debug
{% include video.html src="/env/paint-mode/PaintMode_Debug_Web.mp4" %}

### Setup - PaintMode
![Setup_PaintMode](Setup_PaintMode.png)

- The Mesh you want to paint must be under `Base_Terrain`, or under a Parent that has the `Env Dashboard` script attached
  - Path : Assets/ZLZ_EnvironmentShader/Demo/Prefab/Base_Terrain.prefab
- Assign a Material that uses ZLZ_Environment_Shader
  - Path : Assets/ZLZ_EnvironmentShader/Shaders/Core/ZLZ_Environment_Shader.shader

### ระบบหญ้าของ ZLZ Env Shader ทำอะไรได้บ้าง?
- หญ้าถูกควบคุมผ่าน ZLZ_EnvDashboard (เลือกพื้นผิว, เลือก Grass Type, Paint, Grow) สามารถสร้างหญ้าได้ภายในคลิ๊กเดียว
- ข้อมูลหญ้าถูกเก็บใน ZLZ_EnvGrassData ไม่ใช่บน Scene  ทำให้ Scene เล็ก และหญ้าจะอยู่ใน Prefab  ฉะนั้นจะรองรับการทำงานทั้งแบบ Scene และแบบ Prefab
- สร้างหญ้าขึ้นอัตติโนมัติ โดยเลือกพื้นที่ที่จะเกิดหญ้าจาก Mask Texture ผู้ใช้งานไม่จำเป็นต้องมานั่ง Paint หรือ ระบายเอง
- ผู้ใช้งานสามารถ Paint Grass เพิ่มเติมเองได้หากต้องการ
- รองรับการใช้งานหญ้า + ดอกไม้ ได้หลากหลายประเภทพร้อมกัน
- ลดขนาดหญ้าในบริเวณเส้นขอบ  ระหว่างจุดที่มีหญ้าและไม่มีหญ้า  ทำให้หญ้าเรียบเนียน
- สามารถสร้างพื้นที่ที่ไม่มีหญ้าได้ ผ่านระบบ Blocking Layer
- หญ้ามีระบบ Interactive กับตัวละคร
- หญ้าทำงานคู่กับระบบลม ZLZ_Global_Wind และการทำงานของหญ้าจะ Sync กับลม
- หญ้ารับค่าสีจากกล้อง  ทำให้สีของหญ้าตรงกับสีพื้นตลอดเวลา และถูก Optimize กล้องอย่างดีทำให้กล้องแทบไม่ส่งผลต่อ Performance
- รองรับการใช้งานบนทั้ง PC และ Mobile
- มีระบบ (LOD 0 = Mesh ปกติ, LOD 1 = Optimize Mesh, Culling = นำหญ้าออกหากอยู่ไกลเกินไป)
- หญ้าใช้ GPU Instancing และได้รับการ Optimize เยอะมาก  เพื่อให้ได้ FPS สูงที่สุด

### หญ้ารับค่าสี Realtime ผ่านกล้อง Orthographic

- 


### ระบบ หญ้า Interaction กับตัวละคร
{% include video.html src="/env/grass/Grass_Interaction.mp4" %}

หญ้าจะตอบสนองเมื่อมีตัวละครหรือวัตถุเคลื่อนที่ผ่าน

- ติดตั้ง `ZLZ_Env Grass Interactor` ให้กับตัวละครที่ต้องการ Interaction กับหญ้า
- Radius : ปรับขนาดของพื้นที่ให้เหมาะสมกับตัวละคร  เพื่อ Interaction กับหญ้า
- Push : กำหนดว่าใบหญ้าจะเอนออกด้านข้าง หนีออกจากวัตถุ มากแค่ไหน
- Flatter : กำหนดว่าใบหญ้าจะถูกกดลงล่าง เข้าหาพื้น มากแค่ไหน
- Debug Mode : Interaction เพื่อเช็คพื้นที่ที่ส่งผลกับหญ้า

### ระบบ LOD
{% include video.html src="/env/grass/Grass_LOD_Web.mp4" %}

![Grass_LOD](Grass_LOD.png)

- สามารถ Optimize หญ้าได้ที่ `ZLZ_Global_Grass`
- Performance บอกข้อมูลของหญ้าที่มีในฉากปัจจุบัน
- ผู้ใช้สามารถปรับได้ว่าในแต่ละระยะจะแสดงผล LOD ไหน
- LOD 0 = Mesh หญ้าปกติ
- LOD 1 = Low Mesh + ลดจำนวน
- Culled = ตัดหญ้าออก
- Debug LOD Distance จะแสดงผลในแต่ละระยะ
- LOD 1 Density = ปรับจำนวนของหญ้าที่อยู่ในระยะ LOD 1 ว่าจะลดลงมากแค่ไหน
- มีระบบ Distance Fade เพื่อทำให้การสวิช LOD ในแต่ละระยะเกิดขึ้นอย่างเรียบเนียน
