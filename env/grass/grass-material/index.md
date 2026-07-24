---
layout: docs
title: Grass Material
last_modified_at: 2026-07-24
published: false
---

# Grass Material

Material ของหญ้าคือที่ที่คุมหน้าตาทั้งหมดของใบหญ้า ตั้งแต่สี การไล่เฉด แสงเงา ไปจนถึงเงาที่ทอดลงพื้น ทุกอย่างจัดเป็น section เปิด/ปิดได้ผ่านตาราง Feature ด้านบนสุด เปิดเฉพาะสิ่งที่ใช้เพื่อให้เบาที่สุด

จุดเด่น: หญ้าทั่วไป **ไม่จำเป็นต้องมี Albedo Texture เลย** — ใช้แค่รูปทรงใบ (alpha) บวกกับ Height Gradient ก็ได้สีที่ไล่จากโคนถึงปลายสวยงามและเบามาก

## Showcase
<!-- TODO: ใส่วิดีโอ/รูปตัวอย่าง material ของหญ้า -->
![Grass Material](../GrassMaterial.png)

## Features
ด้านบนสุดของ material เป็นตารางสวิตช์เปิด/ปิดแต่ละความสามารถ (Distance Fade, Wind, Wind Gust Wave, Ground Color, Interaction ฯลฯ) — feature ที่ปิดจะถูกตัดออกจากการคำนวณจริง จึงไม่มีต้นทุน เปิดเฉพาะที่ใช้

## Rendering
- **Cast Shadow** — ให้ใบหญ้าทอดเงาลงพื้นหรือไม่ ปิดได้เพื่อประหยัดในฉากที่มีหญ้าหนาแน่นมาก
- **Alpha Cutoff** — ค่าตัดขอบรูปทรงใบ (ค่าเริ่มต้น `0.6`) ตั้งไว้สูงกว่า 0.5 เล็กน้อยโดยตั้งใจ เพราะเมื่อมี mipmap ขอบ alpha ของใบจะเฉลี่ยเข้าใกล้ 0.5 ทำให้หญ้าไกลๆ กะพริบ การยกไปที่ 0.6 ช่วยตัดอาการกะพริบนั้นออก (ถ้า texture ของคุณต่างออกไปสามารถปรับลดเองได้)

## Texture
เลือกได้ 2 โหมดผ่านแท็บ:

- **Grass Mode** — ใช้ `Blade Albedo (RGB) Alpha (A)` เป็นรูปทรงและสีของใบหญ้าตามปกติ (ส่วนใหญ่ใช้แค่ช่อง alpha เป็นทรงใบ แล้วปล่อยให้ Height Gradient เป็นตัวให้สี)
- **Flower Mode (2D Array)** — สลับใบไปใช้ **หนึ่งสไลซ์**จาก Flower Texture2DArray แทน เพื่อให้ดอกไม้หลายแบบใช้ material และ texture binding ร่วมกันได้ที่ความละเอียดเต็ม
  - **Flower Array (2D Array)** — ใส่ array เดียวกันให้ทุก material ดอกไม้
  - **Flower Slice Index** — แต่ละ material เลือก index สไลซ์ของตัวเอง

## Base Colors
- **Base Color** — สีหลักของใบหญ้าในส่วนที่โดนแสง
- **Shadow Color** — สีของใบหญ้าในส่วนที่อยู่ในเงา

### Height Gradient
ไล่สีใบจากโคนถึงปลายตามความสูงของใบ (ปิดได้ด้วยการตั้งสองสีให้เหมือนกัน) การตั้งค่าหญ้าแบบมาตรฐานมักใช้แค่ alpha ของใบ + gradient นี้ในการให้สี จึงไม่ต้องใช้ albedo texture

- **Gradient** — เปิด/ปิดการไล่เฉดตามความสูง
- **Gradient Bottom** — สีที่โคนใบ (รองรับ HDR)
- **Gradient Top** — สีที่ปลายใบ (รองรับ HDR)
- **Gradient Power** — ความโค้งของการไล่สี ค่าต่ำ = ไล่นุ่มทั่วใบ, ค่าสูง = สีปลายเด่นเฉพาะช่วงยอด

## Lighting
- **Receive Shadow** — ให้ใบหญ้ารับเงาจากวัตถุอื่นหรือไม่
- **Shadow Edge Softness** — ความนุ่มของขอบเงาบนใบ ค่าต่ำ = ขอบเงาคมแบบ toon, ค่าสูง = ไล่นุ่ม
- **Additional Light Intensity** — น้ำหนักที่หญ้ารับจากไฟดวงอื่นๆ (Point/Spot) นอกเหนือจากไฟหลัก

> หมายเหตุ: หญ้า **ไม่รับ** Screen-Space AO โดยตั้งใจ เพราะทุ่งหญ้า alpha-tested หนาแน่นจะกลายเป็นรอยเปื้อนสกปรกใต้ SSAO ใบจึงคงการไล่แสงแบบ toon ที่สะอาดให้เข้าชุดกับ character shader

## Distance Fade
- **Distance Fade** — ค่อยๆ เจือจางหญ้าให้บางลงเมื่อเข้าใกล้ระยะสุดขอบการวาด ผ่าน dither เพื่อให้จางแบบเนียน ไม่ใช่ตัดเป็นวงแข็งๆ ระยะจริงถูกกำหนดจาก Grass Global เพื่อให้ตรงกับระยะ Cull เสมอ — ดูรายละเอียดที่หน้า [Grass LOD]({{ '/env/grass/grass-lod/' | relative_url }})

## Debug
- **Debug Mode** — เลือกข้ามการ shading เพื่อตรวจแต่ละขั้น เช่น `Shadow`, `NdotL` (ค่าไฟหลักดิบๆ), `Ground Color` ตั้งเป็น `Off` เมื่อใช้งานจริง (โหมด debug ของ Wind / Interaction / Ground Color อธิบายไว้ในหน้าของฟีเจอร์นั้นๆ)

---

**ดูเพิ่มเติม:** [Grass Setup]({{ '/env/grass/grass-setup/' | relative_url }}) · [Grass Color Camera]({{ '/env/grass/grass-color-camera/' | relative_url }}) · [Grass Edges]({{ '/env/grass/grass-edges/' | relative_url }}) · [Grass Interaction]({{ '/env/grass/grass-interaction/' | relative_url }}) · [Grass LOD]({{ '/env/grass/grass-lod/' | relative_url }})
