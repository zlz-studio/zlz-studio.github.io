---
layout: docs
title: Grass Setup
last_modified_at: 2026-07-23
published: false
---

# Grass Setup

ปลูกหญ้าลงบน Terrain ของคุณได้ในไม่กี่ขั้นตอน ทุกอย่างคุมผ่าน `ZLZ_Env Dashboard` ที่เดียว และรองรับหญ้าหลายชนิดในฉากเดียว

## Showcase
[(Grass Setup — Plant Different Grass Types on Each Terrain)](https://www.youtube.com/watch?v=3qydgQaQi_s)

## ขั้นตอนการติดตั้ง

![GrassSetup](../GrassSetup.png)

1. **วาง Dashboard** — ใส่ `ZLZ_Env Dashboard` ไว้ที่ root ของ environment (อยู่เหนือ mesh พื้นทั้งหมด) ทุก mesh ที่อยู่ใต้มันจะปรากฏในแผงพร้อมสวิตช์ On/Off
2. **เลือกพื้นที่จะปลูก** — ติ๊กเปิด mesh ที่ต้องการให้มีหญ้า
3. **Source** — เลือกว่าจะให้หญ้าขึ้นบริเวณไหนของ Mesh
   - **Uniform** : ปลูกทั่วทั้ง Mesh
   - **Painted (Mask)** : ปลูกหญ้าเฉพาะบริเวณที่ Paint ไว้บนพื้น โดยอ้างอิงจาก Texture Paint ของ Material พื้น เช่น ถ้าพื้นถูก Paint เป็นโซนหญ้าและโซนทราย หญ้าจะขึ้นเฉพาะโซนหญ้า ไม่ขึ้นบนทราย
4. **Grass Type** — เลือกหรือสร้าง Grass Type เรามี preset ให้พร้อมใช้ดังนี้
   - หญ้า 5 แบบ
   - ดอกไม้สำหรับวางนอน 4 แบบ
   - ดอกไม้สำหรับวางตั้ง 1 แบบ 4 สี
   - หรือผู้ใช้สร้าง Grass Type ของตัวเองได้
5. **Grow All** — กดปุ่ม `Grow All` หญ้าจะถูกสร้างขึ้นตามที่ตั้งไว้
6. **Paint เพิ่ม (ถ้าต้องการ)** — ต้องกด `Grow All` ก่อน แล้วลากเมาส์บนพื้นเพื่อระบายหญ้าเพิ่มเอง

## การสร้าง Grass Type ของตัวเอง
![Create_GrassType](../Create_GrassType.png)

- กด `Create New` ที่ช่อง Grass Type ใน Dashboard
- ระบบจะสร้างไฟล์ไว้ที่ `Assets/ZLZ_EnvironmentShader/Grass/Types/`
- ปรับค่าต่างๆ ได้ในตัว Grass Type เลย
- **Material** : เลือก Material ของหญ้าหรือดอกไม้ที่ต้องการ (ดูตัวอย่างจากภาพ preview ในตัว asset ได้)
- **Meshes** : ใส่ Mesh สำหรับ LOD 0 (ใส่ได้หลายอัน ระบบจะสุ่มใช้ต่อต้น)
- **Far Mesh** : ใส่ Mesh สำหรับ LOD 1 (ระยะไกล) ถ้าเว้นว่างไว้ ระบบจะใช้ Mesh เต็มของ LOD 0 ทุกระยะ
- **Density (per m2)** : ความหนาแน่นในการปลูกต่อ 1 ตร.ม. ยิ่งมากหญ้ายิ่งแน่น
- **Max Tufts** : เพดานจำนวนสูงสุดของ Type นี้ กันไม่ให้หญ้าระเบิดจำนวนบนพื้นผิวใหญ่ๆ
- **Size Min** : ขนาดที่เล็กที่สุดของหญ้า/ดอกไม้
- **Size Max** : ขนาดที่ใหญ่ที่สุด (แต่ละต้นจะสุ่มขนาดระหว่าง Min กับ Max)
- **Height Offset** : ปรับตำแหน่งโคนหญ้า/ดอกไม้ให้จมลงหรือยกขึ้นได้ (ใช้ค่าติดลบเล็กน้อยเพื่อกดโคนให้ติดพื้น ไม่ลอย)
- **Clustering** : ค่ายิ่งสูง หญ้าและดอกไม้จะจับกลุ่มเป็นหย่อมมากขึ้น (0 = กระจายสม่ำเสมอแบบหญ้าทั่วไป) เมื่อเปิด Clustering จะมีสไลเดอร์ `Patch Size` ให้ปรับความกว้างของหย่อม

## ข้อมูลหญ้าเก็บที่ไหน
![GrassSetup](../GrassSetup.png)

หญ้าถูกเก็บใน `ZLZ_EnvGrassData` ซึ่งอยู่ที่ `Assets/ZLZ_EnvironmentShader/Baked/GrassData/`
- Grass Data 1 ไฟล์ ทำงานต่อ 1 Dashboard เท่านั้น หากใช้ Grass Data เดิมปลูกซ้ำ ข้อมูลเก่าจะถูกทับทันที
- กดปุ่ม `New` เพื่อสร้าง Grass Data ไฟล์ใหม่ได้เอง
