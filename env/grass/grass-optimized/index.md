---
layout: docs
title: Grass Optimized
last_modified_at: 2026-07-23
published: false
---

# Grass Optimized

## Showcase เปรียบเทียบ Texture VS Mesh ( Render Scale 1 )
https://www.youtube.com/watch?v=1Nu8tF2xBtI

## Showcase เปรียบเทียบ Texture VS Mesh ( Render Scale 2 )
https://www.youtube.com/watch?v=TuCkT8qo7Ww

---

## ZLZ Grass Mesh Baker

https://www.youtube.com/watch?v=FSAfKJ4hOb0

Mesh + Texture ที่มี Alpha  พื้นที่ส่วนใหญ่เป็นพิกเซลโปร่งใส  แต่ GPU ต้องประมวลผลทุกพิกเซล (fetch texture → ทดสอบ alpha clip → ทิ้งพิกเซล)
ดังนั้นจึงทำให้เกิด Overdraw แต่ ZLZ Grass Mesh Baker จะมาแก้ไขสิ่งนี้

**ได้อะไร**
- ไม่มี Texture fetch
- ไม่มี Alpha Clip
- ไม่มี Overdraw จากพิกเซลโปร่งใส
- Height Gradient + Wind ยังทำงานเหมือนเดิม (คง UV ไว้)
- ผู้ใช้ไม่ต้องปั้น Mesh เอง — แค่มีรูปทรงหญ้าที่ต้องการ กด Bake ก็ได้ Mesh พร้อมใช้ทันที

### การเปิดใช้งาน
Window > ZLZ > Grass Mesh Baker

### หลักการทำงาน 3 ขั้น:

- **Trace** — อ่านช่อง alpha ของ texture แล้ววาดเส้นขอบ (outline) ของใบแต่ละใบ ลดจำนวนจุดตามงบ vertex ที่ตั้ง
- **Layout** — อ่าน mesh อ้างอิง (การ์ดกลุ่มเดิมที่หญ้าใช้อยู่) ทีละการ์ด แล้วแทนทุกการ์ดด้วยรูปที่ trace มา ที่ตำแหน่ง/การหมุน/สเกลเดิมเป๊ะ (ก็อปปี้เลย์เอาต์เดิม ไม่ได้สร้างใหม่)
- **UV** — คง UV แบบ texture-space (x แนวขวาง, y โคน 0 → ปลาย 1) ดังนั้น Height Gradient และ Wind ยังทำงานเหมือนเดิม

### ค่าที่ปรับได้:
- **Grass Texture** — texture ต้นทาง (อ่านแค่ช่อง alpha)
- **Layout Mesh** — mesh การ์ดกลุ่มเดิม (เช่น `SM_Grass_Group1`) ทุกการ์ดจะถูกแทนด้วยรูปที่ trace มา
- **Alpha Cutoff** (`0.05–0.95`) — ระดับ alpha ที่นับเป็นเนื้อใบ ตั้งให้ **ตรงกับ Alpha Cutoff ของ material** ขอบ mesh จะตกตรงที่ขอบ clip เดิม
- **Simplify** (`0.5–8` พิกเซล) — ความละเอียดเส้นขอบ สูง = จุดน้อย/ขอบหยาบ/เบา, ต่ำ = ขอบเนียน/จุดเยอะ
- **Bend Segments** (`1–12`) — แถวจุดแนวนอนโคนถึงปลาย ลมดัด mesh ทีละจุด แถวเยอะ = ลู่ลมเนียน, แถวน้อย = เบาแต่แข็ง
- Preview โชว์เส้นขอบที่ trace (สีทอง) + เส้นแบ่ง Bend Segments พร้อมสถิติ Cards / Verts / Tris ก่อนกด **Bake Mesh**

> หลัง bake: สามารถนำ Mesh ไปใช้ คู่กับ Material ที่ปิดการทำงานของ Texture ได้เลย  สามารถดูตัวอย่างได้จาก Demo

---

## ZLZ Env Grass Resolution

![Grass_Resolution](../grass-optimized/Grass_Resolution.png)

เรนเดอร์ "สีของหญ้า" ลงบัฟเฟอร์ความละเอียดต่ำ แล้ว composite กลับ — จ่ายค่า overdraw ที่เหลือแค่เศษเสี้ยวของพิกเซล

### Setup ZLZ Env Grass Resolution
- Add Renderer Feature > **ZLZ Env Grass Resolution** ลงใน URP หลักที่ใช้ render ภาพ
- ต้องเปิด **Depth Texture** ใน URP Asset

### Resolution

- **Full** — เรนเดอร์เต็มความละเอียด (feature พัก หญ้าวาดสีเอง) → คุณภาพสูงสุด/ขอบคมสุด แต่ไม่ประหยัด
- **Half** — ครึ่งต่อแกน (= 1/4 พิกเซล) → เร็วขึ้นมาก ขอบนุ่มลงเล็กน้อย คุ้มสุดสำหรับส่วนใหญ่
- **Quarter** — 1/4 ต่อแกน (= 1/16 พิกเซล) → เร็วสุด ขอบนุ่มสุด เหมาะกับ mobile หรือฉากหญ้าหนาแน่นจัด
