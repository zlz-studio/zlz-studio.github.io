---
layout: docs
title: Grass Setup
last_modified_at: 2026-07-23
published: false
---

# Grass Setup

ปลูกหญ้าลงบน Terrain ของคุณได้ในไม่กี่ขั้นตอน ทุกอย่างคุมผ่าน `ZLZ_Env Dashboard` ที่เดียว และรองรับหญ้าหลายชนิดในฉากเดียว

## Showcase
(วิดีโอ Grass Setup — 4 Terrain, 4 ชนิดหญ้า)

## ขั้นตอนการติดตั้ง

![GrassSetup](../GrassSetup.png)

1. **วาง Dashboard** — ใส่ `ZLZ_Env Dashboard` ไว้ที่ root ของ environment (อยู่เหนือ mesh พื้นทั้งหมด) — ทุก mesh ที่อยู่ใต้มันจะโผล่ในแผงพร้อมสวิตช์ On/Off
2. **เลือกพื้นที่จะปลูก** — ติ๊กเปิด mesh ที่ต้องการให้มีหญ้า
3. **Source** — เลือกว่าจะหญ้าขึ้นที่บริเวณไหนของ Mesh บ้าง
  - Uniform : ปลูกทั้ง Mesh
  - Painted Mask : ปลูกหญ้าโดยอิงตาม Paint Mode ตัวอย่างเช่น
    - Main Texture = Texture หญ้า
    - Mask Texture R Channel = Texture ทราย
    - หากเราเลือก Main Texture หญ้าจะขึ้นแค่ในพื้นที่หญ้าเท่านั้น  จะไม่ขึ้นที่ Texture ทราย
4. **Grass Type** — เลือกหรือสร้าง Grass Type  เรามี Grass Type ให้แล้วดังนี้
  - หญ้าทั้งหมด 5 แบบ
  - ดอกไม้สำหรับวางนอน 4 แบบ
  - ดอกไม้สำหรับวางตั้ง 1 แบบ 4 สี
  - ผู้ใช้งานสร้าง Grass Type ที่ตัวเองต้องการเองได้
5. **Grow All** — กดปุ่ม `Grow All` หญ้าจะถูกสร้างขึ้นตามที่ตั้งไว้
6. **Paint เพิ่ม (ถ้าต้องการ)** — ต้อง Grow All ก่อน แล้วลากเมาส์บนพื้นเพื่อระบายหญ้าเพิ่มเอง

## การสร้าง Grass Type ของตัวเอง
- สามารถสร้างที่ Create New ที่ Grass Type ใน Dashboard
- ระบบจะสร้าง Grass Type ไว้ที่นี่ > Assets/ZLZ_EnvironmentShader/Grass/Types/ZLZ_EnvFlower001.asset
- 

## หญ้าหลายชนิดในฉากเดียว
แต่ละ Grass Type มีส่วน **Grows On** เลือกได้ว่าจะให้ขึ้นบนพื้นผิวไหน — จึงทำให้แต่ละ  Terrain มีหญ้าคนละแบบได้ (อย่างในคลิปที่ 4 Terrain ปลูกหญ้าคนละชนิด)

## ข้อมูลหญ้าเก็บที่ไหน
หญ้าถูกเก็บใน `ZLZ_EnvGrassData` ไม่ฝากไว้บน Scene และติดไปกับ Prefab — รองรับทั้งงานแบบ Scene และ Prefab

## หน้าต่อไป
- [Grass Color Camera] · [Grass Interaction] · [Grass LOD] · [Grass Edges]
