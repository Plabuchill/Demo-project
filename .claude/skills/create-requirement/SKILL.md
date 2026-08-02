---
name: create-requirement
description: รับ requirement ดิบจากผู้ใช้ แปลงเป็นเอกสาร requirement อย่างเป็นทางการที่ docs/01-requirements/01-spec/ อัปเดต docs/01-requirements/backlog.md และบันทึกสรุปงานที่ docs/05-log/ ใช้เมื่อผู้ใช้ให้ requirement/ไอเดียฟีเจอร์ใหม่แบบข้อความอิสระ หรือพิมพ์ /create-requirement
argument-hint: [requirement ดิบ]
---

## เมื่อ skill นี้ถูกเรียก

1. **รวบรวม requirement ดิบ** — ถ้ามี `$ARGUMENTS` ให้ถือเป็น requirement ดิบทั้งหมด ถ้าไม่มี (เรียกลอย ๆ) ให้ถามผู้ใช้ก่อนว่า requirement ดิบคืออะไร อย่าเดาเอง

2. **ตรวจการอ้างอิงเอกสารเดิม** — ถ้า requirement ดิบพูดถึงฟีเจอร์/เอกสารที่อาจมีอยู่แล้ว ให้ลอง `Glob`/`Grep` หาไฟล์ที่เกี่ยวข้องใน `docs/01-requirements/01-spec/` ก่อน แล้วแนบชื่อไฟล์ที่พบ (ถ้ามี) ไปให้ agent ในขั้นตอนถัดไปด้วย

3. **มอบหมายให้ agent `requirement-writer`** ผ่าน Agent tool โดย **ต้องรันแบบ foreground** (`run_in_background: false`) เพราะ agent นี้อาจต้องถามคำถามกลับมาที่ผู้ใช้ระหว่างทำงาน (ผ่าน `AskUserQuestion`) prompt ที่ส่งต้องครบถ้วนในตัวเอง (agent ใหม่ไม่เห็นบทสนทนานี้) ประกอบด้วย:
   - requirement ดิบแบบเต็ม (คำต่อคำ)
   - รายชื่อไฟล์ spec ที่อาจเกี่ยวข้อง (ถ้าพบในขั้นตอนที่ 2)
   - ย้ำหน้าที่ทั้งหมด: วิเคราะห์ว่าควรสร้างเอกสารใหม่หรือแก้เอกสารเดิม, ถามผู้ใช้เมื่อไม่แน่ใจโดยต้องมีตัวเลือกอย่างน้อย 3 แนวทางเสมอ, สร้าง/แก้ไขเอกสารที่ `docs/01-requirements/01-spec/{YYYYMMDD}-{RUNNING_NO}-{SUMMARIZE_TOPIC}.md`, อัปเดต `docs/01-requirements/backlog.md`, บันทึกสรุปที่ `docs/05-log/{YYYYMMDD}-log.md`

4. **สรุปให้ผู้ใช้** — เมื่อ agent ทำงานเสร็จ สรุปสั้น ๆ ว่ามีการสร้าง/แก้ไขไฟล์อะไรบ้าง (ลิงก์ไฟล์ทั้งหมดที่เปลี่ยนแปลง)

อย่าทำขั้นตอนการเขียนเอกสาร/อัปเดต backlog/log เองในนี้โดยตรง — ให้เป็นหน้าที่ของ agent `requirement-writer` เสมอ เพื่อให้ logic อยู่ที่เดียว (`.claude/agents/requirement-writer.md`)
