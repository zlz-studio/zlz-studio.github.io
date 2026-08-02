---
layout: docs
title: FX Runtime Overview
last_modified_at: 2026-08-02
published: false
---

<!-- DRAFT — ยังไม่ขึ้นเว็บจริง. พรีวิว: jekyll serve --unpublished. พร้อมขึ้นเว็บ: ลบ published: false -->

# FX Runtime

<!-- TODO: อธิบายว่าหมวดนี้คืออะไร — เอฟเฟกต์ที่ขับด้วยสคริปต์ตอนรัน ไม่ใช่ค่าที่ตั้งทิ้งไว้ในวัสดุ
     ต่างจากหมวด Environment Shader ตรงที่ต้องมี component ในฉากและเรียกจากโค้ดเกม -->

---

## How It Works

<!-- TODO: แนวคิดร่วมของทั้งหมวด
     - ทุกตัวมาจาก ZLZ_EnvFX.hlsl ("Environment VFX Utilities : Darken / Light Sweep / Upgrade")
       ซึ่ง adapted มาจาก ZLZ_CharacterFX.hlsl ของ ZLZ Anime Shader
     - API ฝั่ง Env ตั้งใจให้เหมือนฝั่ง Anime เพื่อให้โค้ดเกมชุดเดียวขับได้ทั้งคู่
     - Target Darken ใช้ร่วมกับ grass / water ได้ด้วย (แยก header เป็น ZLZ_EnvDarken.hlsl) -->

---

## Pages in This Section

<!-- TODO: ลิงก์แต่ละหน้าเมื่อเขียนเสร็จ

- [Target Darken FX Runtime](../fx/target-darken/) — หรี่ทั้งฉากเพื่อขับเป้าหมายให้เด่น
- Light Sweep — แถบแสงกวาดผ่านผิว
- Upgrade — แสงวาบตอนอัปเกรด

-->

---
