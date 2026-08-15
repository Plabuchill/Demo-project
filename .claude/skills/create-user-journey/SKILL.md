---
name: create-user-journey
description: สร้างหรืออัปเดตเอกสาร user journey (พร้อม Mermaid flowchart และคำอธิบายใต้กราฟ) ใน docs/02-design/01-prototypes/ จากเอกสาร spec ที่มีอยู่ใน docs/01-requirements/01-spec/ ใช้เมื่อผู้ใช้ต้องการ map user flow ของ actor ใด ๆ จาก spec ที่มีอยู่ หรือพิมพ์ /create-user-journey
argument-hint: [spec หรือ actor ที่ต้องการสร้าง/อัปเดต user journey]
---

## เมื่อ skill นี้ถูกเรียก

1. **ระบุ spec/actor เป้าหมาย** — ถ้ามี `$ARGUMENTS` ให้ใช้เป็นตัวระบุ spec หรือ actor ถ้าไม่มี (เรียกลอย ๆ) ให้ถามผู้ใช้ก่อนว่าต้องการทำ journey ของ spec/actor ไหน อย่าเดาเอง

2. **หา spec ที่เกี่ยวข้อง** — `Glob`/`Grep` ใน `docs/01-requirements/01-spec/` เพื่อหาไฟล์ spec ที่ตรงกับสิ่งที่ผู้ใช้ระบุ แนบรายชื่อไฟล์ที่พบไปให้ agent ในขั้นตอนถัดไป ถ้าหาไม่พบหรือกำกวม (พบหลายไฟล์ที่เข้าข่าย) ให้แนบทั้งหมดไปแล้วให้ agent เป็นผู้ถามผู้ใช้เพื่อยืนยัน

3. **มอบหมายให้ agent `docs-maintainer`** ผ่าน Agent tool โดย **ต้องรันแบบ foreground** (`run_in_background: false`) prompt ที่ส่งต้องครบถ้วนในตัวเอง (agent ใหม่ไม่เห็นบทสนทนานี้) ประกอบด้วย:
   - **ให้รันโหมด B (สร้าง/อัปเดต user journey)** เท่านั้น ไม่ใช่โหมด A
   - สิ่งที่ผู้ใช้ระบุมาแบบเต็ม (คำต่อคำ) + รายชื่อไฟล์ spec ที่พบในขั้นตอนที่ 2
   - ย้ำหน้าที่ทั้งหมดตามขั้นตอน B1-B8 ใน `.claude/agents/docs-maintainer.md`: ตรวจสอบ journey เดิมก่อนว่ามีอยู่แล้วหรือไม่ (แก้ไข vs สร้างใหม่), สกัด actor+ขั้นตอนจาก spec, ตั้งชื่อไฟล์ตามรูปแบบ `{YYYYMMDD}-{RUNNING_NO}-user-journey-{actor}-{topic}.md`, เขียนเอกสารที่ต้องมี Mermaid flowchart พร้อมคำอธิบายใต้กราฟเสมอ (ไม่ใช่ทางเลือก), อัปเดตตารางใน `docs/02-design/01-prototypes/index.md`, บันทึก log

4. **สรุปให้ผู้ใช้** — เมื่อ agent ทำงานเสร็จ สรุปสั้น ๆ ว่าสร้าง/แก้ไขไฟล์อะไรบ้าง (ลิงก์ไฟล์ทั้งหมด)

อย่าทำขั้นตอนการเขียนเอกสาร/อัปเดต index/log เองโดยตรงในนี้ — ให้เป็นหน้าที่ของ agent `docs-maintainer` เสมอ เพื่อให้ logic อยู่ที่เดียว (`.claude/agents/docs-maintainer.md`)
