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

1. **วาง Dashboard** — ใส่ `ZLZ_Env Dashboard` ไว้ที่ root ของ environment (อยู่เหนือ mesh พื้นทั้งหมด) — ทุก mesh ที่อยู่ใต้มันจะโผล่ในแผงพร้อมสวิตช์ On/Off
2. **เลือกพื้นที่จะปลูก** — ติ๊กเปิด mesh ที่ต้องการให้มีหญ้า
3. **สร้าง Grass Type** — กด `Create New` เพื่อสร้าง preset หญ้า/ดอกไม้ (material + meshes) ที่นำกลับมาใช้ซ้ำได้ — เพิ่มได้หลายชนิดด้วย `+ Add Slot`
4. **Grow All** — กดปุ่ม `Grow All` หญ้าจะถูกสร้างขึ้นตามที่ตั้งไว้
5. **Paint เพิ่ม (ถ้าต้องการ)** — ต้อง Grow All ก่อน แล้วลากเมาส์บนพื้นเพื่อระบายหญ้าเพิ่มเอง
6. **เปิดระบบ Optimize** — กด `Install Grass Performance Feature` เพื่อเปิด LOD/Culling (ดูรายละเอียดที่หน้า Grass LOD)

## หญ้าหลายชนิดในฉากเดียว
แต่ละ Grass Type มีส่วน **Grows On** เลือกได้ว่าจะให้ขึ้นบนพื้นผิวไหน — จึงทำให้แต่ละ  Terrain มีหญ้าคนละแบบได้ (อย่างในคลิปที่ 4 Terrain ปลูกหญ้าคนละชนิด)

## ข้อมูลหญ้าเก็บที่ไหน
หญ้าถูกเก็บใน `ZLZ_EnvGrassData` ไม่ฝากไว้บน Scene และติดไปกับ Prefab — รองรับทั้งงานแบบ Scene และ Prefab

## หน้าต่อไป
- [Grass Color Camera] · [Grass Interaction] · [Grass LOD] · [Grass Edges]
