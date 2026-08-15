---
name: audit-feature-list
description: ตรวจสอบความสอดคล้องระหว่าง backlog.md กับเอกสาร spec ทั้งหมดใน docs/01-requirements/01-spec/ แล้วสร้าง/อัปเดต docs/01-requirements/feature-list.md ที่แตกเป็นรายฟีเจอร์พร้อมจัดกลุ่ม MoSCoW เบื้องต้น (Must/Won't) ใช้เมื่อผู้ใช้ต้องการตรวจสอบ backlog หรือดูรายการฟีเจอร์แบบละเอียด หรือพิมพ์ /audit-feature-list
---

## เมื่อ skill นี้ถูกเรียก

1. มอบหมายให้ agent `docs-maintainer` ผ่าน Agent tool โดย **ต้องรันแบบ foreground** (`run_in_background: false`) เพราะ agent อาจต้องถามคำถามกลับผ่าน `AskUserQuestion` เมื่อพบความกำกวม prompt ที่ส่งต้องระบุชัดเจนว่า:
   - **ให้รันโหมด A (ตรวจสอบและสร้าง feature list)** เท่านั้น ไม่ใช่โหมด B
   - ให้ทำตามขั้นตอน A1-A6 ทั้งหมดตามที่ระบุใน `.claude/agents/docs-maintainer.md`: อ่าน `backlog.md` + ทุกไฟล์ใน `01-spec/`, ตรวจสอบ 4 ประเด็น (เอกสารกำพร้า/ลิงก์ขาด/สถานะไม่ตรง/สมมติฐานค้าง), แตกฟีเจอร์จาก "ขอบเขต (ทำ/ไม่ทำ)" พร้อม MoSCoW (Must/Won't เท่านั้น ห้ามเดา Should/Could เอง), regenerate `docs/01-requirements/feature-list.md` ทั้งไฟล์, บันทึก log ที่ `docs/05-log/{YYYYMMDD}-log.md`

2. **สรุปให้ผู้ใช้** — เมื่อ agent ทำงานเสร็จ สรุปผลตรวจสอบ (เน้นปัญหาที่พบถ้ามี) และลิงก์ไฟล์ที่สร้าง/แก้ไข

อย่าทำการอ่าน/เขียนไฟล์เองโดยตรงในนี้ — ให้เป็นหน้าที่ของ agent `docs-maintainer` เสมอ เพื่อให้ logic อยู่ที่เดียว (`.claude/agents/docs-maintainer.md`)
