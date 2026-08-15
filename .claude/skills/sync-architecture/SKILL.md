---
name: sync-architecture
description: ตรวจสอบและสร้าง/ปรับปรุง docs/02-design/02-technical/architecture.md (hi-level design แบบไม่ผูก tech stack) ให้สอดคล้องกับ docs/01-requirements/feature-list.md และเอกสาร user journey ล่าสุด ใช้เมื่อผู้ใช้ต้องการ "ทำ architecture", "ออกแบบ hi-level design", "เขียน system architecture", "อัปเดต architecture จาก feature list" หรือพิมพ์ /sync-architecture
---

## เมื่อ skill นี้ถูกเรียก

1. มอบหมายให้ agent `architecture-writer` ผ่าน Agent tool โดย **ต้องรันแบบ foreground**
   (`run_in_background: false`) prompt ที่ส่งต้องมี: วันที่ปัจจุบัน (YYYYMMDD) สำหรับใช้กับไฟล์ log
   และบริบทว่านี่คือ session สนทนากับผู้ใช้จริง คำถามที่ agent ถามจะไปถึงผู้ใช้ทันที

2. **ตรวจจับสัญญาณ "ต้องการ requirement ใหม่"**: อ่านรายงานที่ได้กลับมาทั้งหมด
   - ไม่มีหัวข้อ `## NEEDS_NEW_REQUIREMENT` → ข้ามไปข้อ 4
   - มีหัวข้อนี้ → ทำข้อ 3 ก่อน

3. **Auto-chain ไป `requirement-writer` แล้ว refresh feature-list (ไม่เกิน 1 รอบ)**:
   a. แจ้งผู้ใช้สั้นๆ ว่าเจอความต้องการใหม่ระหว่างออกแบบ architecture กำลังส่งต่อให้
      `requirement-writer` อัตโนมัติ (ไม่ต้องถามก่อน)
   b. เรียก agent `requirement-writer` (`run_in_background: false`) พร้อมเนื้อหาใต้หัวข้อสัญญาณ
      แบบ verbatim + วันที่ + บริบท session จริง
   c. รอผลลัพธ์ สุ่มตรวจ `docs/01-requirements/backlog.md` อย่างน้อย 1 จุด
   d. เรียก agent `docs-maintainer` โหมด A (audit + regenerate `feature-list.md`) ให้ feature-list
      สะท้อนฟีเจอร์ใหม่ก่อน แล้วเรียก agent `architecture-writer` อีกรอบ (prompt เดิม)
   e. **ป้องกัน infinite loop**: ทำซ้ำได้อีกไม่เกิน 1 รอบ

4. **ตรวจสอบผลลัพธ์ก่อนรายงาน**: สุ่มอ่าน `architecture.md` จริงอย่างน้อย 1-2 จุด ตรวจว่ามี Mermaid
   diagram จริง และไม่มีชื่อ technology/framework ใดๆ หลุดเข้าไป

5. **สรุปให้ผู้ใช้ทราบ**: up to date อยู่แล้วหรือไม่ก่อนตรวจ, ส่วนที่แก้ไข, ความต้องการใหม่ที่
   auto-chain ไปให้ (ถ้ามี), จุดที่ถามผู้ใช้ (ถ้ามี)

## ข้อควรระวัง

- ห้ามข้ามการเรียก agent แล้วเขียน architecture.md เองตรงๆ ในเทรดหลัก
- Agent นี้ห้ามแตะไฟล์ใดๆ นอกจาก `architecture.md` กับไฟล์ log — หากผลลัพธ์พูดถึงการแก้ไฟล์อื่น
  (โดยเฉพาะ `feature-list.md`, เอกสาร journey, `technology-stack.md`) ให้หยุดและแจ้งผู้ใช้ทันที
- **ถ้าผลลัพธ์ระบุชื่อ technology/framework/database engine ใดๆ ทั้งที่ `technology-stack.md`
  ยังว่างเปล่าอยู่ ให้ถือว่าผิดกฎ** หยุดและแจ้งผู้ใช้ทันที
- ห้ามวน auto-chain เกิน 1 รอบเด็ดขาด
- ถ้า agent รายงานว่า `feature-list.md` ว่างเปล่า/ไม่มี ให้แจ้งผู้ใช้ให้รัน `audit-feature-list` ก่อน
