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

Selection Outline คือ URP Renderer Feature ที่วาด Outline Silhouette แบบ Animate รอบ GameObject ที่ต้องการขณะ Runtime ออกแบบมาสำหรับระบบต่อสู้ การ Lock-on และ Object โต้ตอบได้ — รองรับทั้ง PC และ Mobile

- 3 ประเภทในตัว — Character, Enemy, Item (แต่ละประเภทมีสีเป็นของตัวเอง และรองรับ HDR)
- สามารถปรับสีและความหนาของ Outline ได้
- Animation ลื่นไหล — Intro ค่อยๆ ปรากฏ → Loop พัลส์ → Outro ค่อยๆ หาย
- Setup คลิกเดียว ผ่าน Character Dashboard
- ควบคุมผ่าน Script — เรียก Select() และ Deselect() จาก Script ใดก็ได้
- รองรับการใช้งานกับกล้อง Orthographic

---

การติดตั้ง

![Dashbord_Outline_Selection](../Outline-Selection/Dashbord_Outline_Selection.png)

1. เปิด Character Dashboard ของตัวละครใน Inspector → เลือก Select Type ที่ต้องการ (Character, Enemy, Item)

![Universal Render Data](../Outline-Selection/Universal_Render_Data.png)

2. เปิด Universal Renderer Data จะเจอ ZLZ Selection Outline → ปรับสี + ความหนา + Animation Curve ได้ที่นี่

---

การใช้งานผ่าน Script
เพิ่ม using ZLZ.AnimeShader; แล้วอ้างอิง ZLZ_SelectionController จากนั้นเรียกใช้

> // แบบ Animate (แนะนำ) — เล่น Intro → Loop → Outro  
> ctrl.Select();  
> ctrl.Deselect();  
> ctrl.ToggleSelection();  
>   
> // เช็คสถานะ  
> bool active = ctrl.IsSelected();  
