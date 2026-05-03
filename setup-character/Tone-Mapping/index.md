---
layout: docs
title: Tone-Mapping
---

## Tone Mapping

![outline](../Tone-Mapping/ToneMapping.jpg)

### What is Tone Mapping?

Tone Mapping is the process of adjusting the brightness of an image so it can be properly displayed on screen.

---

### What does Tone Mapping actually fix?

Tone Mapping is not just about “reducing brightness”
It is about reshaping how light behaves across the entire image

What Tone Mapping controls:
- How quickly highlights turn white
- How deep shadows appear
- Whether colors are preserved or desaturated
- The overall contrast of the image

---

### Built-in Tone Mapping vs ZLZ Tone Mapping

By default, Unity provides two main tone mapping options: Neutral and ACES

### Neutral

![outline](../Tone-Mapping/Netural.jpg)

Neutral tone mapping is designed to preserve the original color response as much as possible.

Characteristics:
- Preserves color relatively well in high-intensity areas
- Produces a softer, slightly washed-out look
- Lower contrast, which can make the image feel less sharp or slightly dull

### ACES

![outline](../Tone-Mapping/ACES.jpg)

ACES is designed to produce a more cinematic and physically-inspired result.

Characteristics:
- Higher contrast with a strong cinematic look
- Highlights tend to shift toward white under intense lighting
- Shadows can become overly dark (crushed)
- Very bright areas may appear too intense or lose detail

---

### ZLZ Tone Mapping
ZLZ Tone Mapping provides two curve options:

### 1. Anime Curve (Recommended)

![outline](../Tone-Mapping/Anime.jpg)

Designed to preserve the visual quality of Anime-style rendering

Key features:
- Maintains a sharp and well-defined image
- Preserves color even in high-intensity lighting
- Enhances color vibrancy for a more appealing look
- Highlights do not turn white too quickly or too aggressively
- Shadows remain readable and do not become overly crushed

### 2. Filmic Curve

![outline](../Tone-Mapping/Filmic.jpg)

Designed for a more cinematic and natural-looking result

Key features:
- Produces a smoother and more balanced image (less flat than Neutral)
- Preserves color better than ACES in bright areas
- Colors look good but are less saturated than Anime Curve
- Highlights are controlled and do not blow out too quickly
- Shadows remain softer and more natural

---

### Setup Tone Mapping

![outline](../Tone-Mapping/Setup-ToneMapping.jpg)

1. Go to Project Settings > Quality, Under Render Pipeline Asset, select the active URP Asset  
2. Select the Universal Renderer Data, then add the ZLZ Anime ToneMapping feature.  
3. Create a Global Volume and add ZLZ Anime ToneMapping and Bloom overrides.

---

### Using Tone Mapping
- By default, ZLZ Anime Shader provides two tone mapping options: Anime Curve and Filmic Curve
- You can adjust the parameters to fine-tune the image based on your desired art direction
- While adjusting, you can preview the result using the Curve Preview in the Editor in real-time
- After tuning, you can click Save as My Default to store your settings, and use Reset to My Default to restore them at any time
- If needed, you can click Restore Factory to revert back to the original default settings

---

### ทำความรู้จัก Parameters Anime Curve

- Exposure : ปรับความสว่างโดยรวมของภาพ
- Tone Map Scale : เพิ่ม/ลดความแรงของ Tone Mapping ก่อนเข้าสู่ curve → ใช้ร่วมกับ Exposure เพื่อบาลานซ์ภาพ
- Highlight Rolloff : ควบคุมความเร็วในการไล่ระดับของ Highlight ไปสู่ความสว่างสูงสุด
- Highlight Lift : เพิ่มความสว่างของ Highlight โดยรวม ทำให้ส่วนสว่างดูเด่นขึ้น
- White Clip : กำหนดขอบเขตของ Highlight ว่าจะไปถึงความสว่างสูงสุดได้แค่ไหน
- Shadow Pedestal : ยกระดับ Shadow → ป้องกันภาพดำสนิทเกินไป
- Mid Contrast : เพิ่ม Contrast ในช่วงกลางของภาพ → ทำให้ภาพดูคมขึ้นโดยไม่กระทบ Highlight มาก
- Saturation Retention : รักษาความอิ่มสีในช่วงที่สว่าง → ลดปัญหาสีซีดใน Highlight

---

### ทำความรู้จัก Parameters Filmic Curve

- Exposure : ปรับความสว่างโดยรวมของภาพ
- Tone Map Scale : เพิ่ม/ลดความแรงของ Tone Mapping ก่อนเข้าสู่ curve → ใช้ร่วมกับ Exposure เพื่อบาลานซ์ภาพ
- Shoulder Strength : ควบคุมการบีบของ Highlight → ทำให้ส่วนสว่างไม่พุ่งขาวเร็ว และไล่ระดับได้นุ่มขึ้น
- Linear Strength : ควบคุมความชัดของช่วงกลางภาพ → มีผลต่อ contrast โดยรวมของภาพ
- Linear Angle : ปรับลักษณะการไล่ระดับของแสงในช่วงกลาง → ช่วยบาลานซ์ระหว่างส่วนกลางและส่วนสว่าง
- Toe Strength : ควบคุมการไล่ระดับของ Shadow → ทำให้เงาดูเนียนและไม่ตัดแข็ง
- Black Level : ยกระดับสีดำ → ป้องกันภาพดำสนิทเกินไป
- Denominator Balance : ปรับรูปทรงของ curve โดยรวม → ส่งผลต่อการกระจายแสงทั้งภาพ
- White Point : กำหนดระดับความสว่างสูงสุดของภาพ → คุมช่วงการแสดงผลของ Highlight
- Saturation : ปรับความอิ่มสีหลัง Tone Mapping → ช่วยควบคุมความสดของภาพโดยรวม
