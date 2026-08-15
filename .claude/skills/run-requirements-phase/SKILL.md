---
name: run-requirements-phase
description: รวมขั้นตอนช่วง requirement ทั้งหมดในคำสั่งเดียว - (ถ้าผู้ใช้แนบ requirement ดิบมาด้วย) create-requirement → spec ↔ backlog/feature-list → user journey → เอกสารทดสอบ (acceptance-criteria.md/test-plan.md/test-cases/) แล้วรายงาน output แยกตาม sub-step ในคำตอบเดียว ใช้เมื่อผู้ใช้ต้องการ "ทำเอกสารช่วง requirement ทั้งหมด", "จาก requirement ถึง test plan ในคำสั่งเดียว", "รัน phase 1" หรือพิมพ์ /run-requirements-phase
---

## เมื่อ skill นี้ถูกเรียก

Skill นี้เป็น orchestrator สำหรับช่วง requirement ทั้งหมด: requirement → backlog + feature-list →
user journey → test case/acceptance criteria/test plan โดยเรียก skill/agent ที่มีอยู่แล้วเรียง
ตามลำดับ ไม่เขียนไฟล์เอง รองรับ entry point แบบ "มี requirement ดิบใหม่มาด้วย" ไม่ใช่แค่โหมด audit
เอกสารที่มีอยู่แล้ว

1. **หาวันที่ปัจจุบัน**: รูปแบบ `YYYYMMDD` เพื่อส่งต่อให้ skill/agent ทุกตัวใช้กับไฟล์ log

2. **ชั้นที่ 1 — spec ↔ backlog ↔ feature-list**: ตรวจก่อนว่าคำขอของผู้ใช้ในเทิร์นนี้แนบ
   requirement ดิบมาด้วยหรือไม่
   - **ถ้ามี**: เรียกผ่าน **Skill tool** ด้วย `skill: create-requirement` พร้อม requirement ดิบ
     verbatim ทั้งหมด (skill นี้จัดการทั้งสร้าง/แก้ไข spec และอัปเดต backlog อยู่แล้ว) แล้วเรียก
     agent `docs-maintainer` โหมด A ต่อทันทีเพื่อ regenerate `feature-list.md` ให้ครอบคลุมรหัสใหม่
   - **ถ้าไม่มี** (ผู้ใช้แค่ต้องการให้ไล่ sync เอกสารที่มีอยู่แล้วให้ตรงกันทั้งช่วง): เรียกผ่าน
     **Skill tool** ด้วย `skill: audit-feature-list` (ตรวจ spec ↔ backlog ↔ feature-list ในคำสั่ง
     เดียวอยู่แล้ว)
   รอผลลัพธ์ก่อนไปขั้นตอนถัดไปเสมอ

3. **ตรวจสอบผลลัพธ์ชั้นที่ 1**: สุ่มอ่าน `docs/01-requirements/feature-list.md` จริงอย่างน้อย 1-2
   จุดก่อนไปต่อ ถ้าพบปัญหาเชิงโครงสร้างที่ทำให้ตรวจต่อไม่ได้ ให้หยุด pipeline ทันทีและรายงานผู้ใช้

4. **ชั้นที่ 2 — user journey**: ถ้าฟีเจอร์ Must ใน feature-list.md ยังมีบางส่วนไม่มีเอกสาร journey
   รองรับ ให้เรียกผ่าน **Skill tool** ด้วย `skill: create-user-journey` ต่อ (ระบุ actor/หัวข้อที่
   ยังขาดจากการอ่านในข้อ 3) **ต้องรอชั้นที่ 1 เสร็จสมบูรณ์ก่อนเสมอ**

5. **ตรวจสอบผลลัพธ์ชั้นที่ 2**: สุ่มอ่านเอกสาร journey ที่สร้าง/แก้ไขจริง (รวมถึงตรวจว่ามี mermaid
   block กำกับ) ถ้าพบปัญหาเชิงโครงสร้าง ให้หยุด pipeline และรายงานผู้ใช้เช่นกัน

6. **ชั้นที่ 3 — เอกสารทดสอบ**: เรียกผ่าน **Skill tool** ด้วย `skill: sync-test-plan` (มี
   auto-chain guard ไป `requirement-writer` ในตัวอยู่แล้วเผื่อเจอ requirement ใหม่ระหว่างเขียน
   acceptance criteria) **ต้องรอชั้นที่ 2 เสร็จสมบูรณ์ก่อนเสมอ**

7. **ตรวจสอบผลลัพธ์ชั้นที่ 3**: สุ่มอ่าน `acceptance-criteria.md`, `test-plan.md`, และไฟล์ใน
   `test-cases/` จริงอย่างน้อย 1-2 จุด ถ้า `sync-test-plan` รายงานว่า auto-chain ไป
   `requirement-writer` ระหว่างทาง ให้บันทึกไว้สำหรับสรุปในข้อ 8 ด้วย (เป็นเรื่องปกติ ไม่ใช่
   ข้อผิดพลาด)

8. **สรุปผลรวมทั้งช่วงเป็นรายงานเดียว** แยกตาม sub-step: spec/backlog/feature-list (สร้าง/แก้ไข
   อะไรบ้าง), user journey (เอกสารที่สร้าง/แก้ไข), เอกสารทดสอบ (up to date หรือแก้ไขอะไรไปบ้าง
   รวมความต้องการใหม่ที่ auto-chain ไปให้), จุดที่ถามผู้ใช้ระหว่างทาง (ถ้ามี)

## ข้อควรระวัง

- **ต้องรันตามลำดับชั้นเสมอ (sequential) ห้ามรันขนาน**
- ห้ามข้ามการเรียก skill/agent แล้วแก้ไฟล์เองตรงๆ ในเทรดหลักไม่ว่าชั้นใด
- Skill นี้ scope แค่ "ช่วง requirement" เท่านั้น — **ไม่ลากต่อไปช่วง prototype
  (`run-prototype-phase`) หรือช่วง technical/phase plan (`run-technical-phase`)** แม้ผลลัพธ์จะมี
  ฟีเจอร์ใหม่เกิดขึ้น ให้จบแค่รายงานสรุปแล้วแนะนำผู้ใช้ว่าถ้าต้องการอัปเดต prototype/เอกสารเทคนิคต่อ
  ให้เรียก skill ที่เกี่ยวข้องเพิ่มเอง
- `sync-test-plan` มี loop guard ของตัวเองอยู่แล้ว ไม่ต้องทำ guard ซ้ำในชั้นนี้
- ถ้า skill/agent ตัวใดตัวหนึ่งรายงานปัญหาเชิงโครงสร้าง ให้หยุด pipeline ทันที ไม่ไปชั้นถัดไป แล้ว
  รายงานปัญหานั้นให้ผู้ใช้ก่อนเสมอ
- ถ้าผู้ใช้ส่ง requirement ดิบมาหลายเรื่องไม่เกี่ยวข้องกันในคำขอเดียว ปล่อยให้
  `create-requirement`/`requirement-writer` จัดการถามผู้ใช้เอง อย่าตัดสินใจแทน
