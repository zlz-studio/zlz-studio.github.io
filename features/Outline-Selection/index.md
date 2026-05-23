---
layout: docs
title: Outline-Selection
last_modified_at: 2026-05-23
---

## Outline Selection

![OutlineSelection](../Outline-Selection/OutlineSelection.gif)

มอบ Visual Feedback ให้ผู้เล่นได้ทันที — ไฮไลต์ตัวละคร ศัตรู หรือ Item ด้วย Outline แบบ Animate ที่พัลส์และจางออกอย่างสวยงาม

---

ภาพรวม

![Universal Render Data](../Outline-Selection/Universal Render Data.png)

Selection Outline คือ URP Renderer Feature ที่วาด Outline Silhouette แบบ Animate รอบ GameObject ที่ต้องการขณะ Runtime ออกแบบมาสำหรับระบบต่อสู้ การ Lock-on และ Object โต้ตอบได้ — รองรับทั้ง PC และ Mobile

3 ประเภทในตัว — Character, Enemy, Item (แต่ละประเภทมีสีเป็นของตัวเอง)
Animation ลื่นไหล — Intro ค่อยๆ ปรากฏ → Loop พัลส์ → Outro ค่อยๆ หาย
Setup คลิกเดียว ผ่าน Character Dashboard
ควบคุมผ่าน Script — เรียก Select() และ Deselect() จาก Script ใดก็ได้

### Parameters

- **Normal Strength :** Controls the intensity of the Normal Map and how strongly it affects the surface details

### Preview NormalMap Pass

![NormalMap_Pass](../NormalMap/NormalMap_Pass.png)
