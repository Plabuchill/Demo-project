---
name: sync-api-db
description: ตรวจสอบและสร้าง/ปรับปรุง docs/02-design/02-technical/api-spec.md และ db-spec.md (เขียนคู่กันเสมอ แบบไม่ผูก tech stack) ให้สอดคล้องกับ architecture.md ล่าสุด ใช้เมื่อผู้ใช้ต้องการ "ทำ API spec", "ออกแบบ database schema", "เขียน db-spec", "อัปเดต api spec จาก architecture" หรือพิมพ์ /sync-api-db
---

## เมื่อ skill นี้ถูกเรียก

1. มอบหมายให้ agent `api-db-writer` ผ่าน Agent tool (`run_in_background: false`) พร้อมวันที่
   ปัจจุบัน (YYYYMMDD) และบริบทว่านี่คือ session สนทนากับผู้ใช้จริง

2. **ตรวจจับสัญญาณ "ต้องการ requirement ใหม่"**: ไม่มีหัวข้อ `## NEEDS_NEW_REQUIREMENT` → ข้ามไป
   ข้อ 4 มีหัวข้อนี้ → ทำข้อ 3 ก่อน

3. **Auto-chain ไป `requirement-writer` (เฉพาะเมื่อมีสัญญาณ)**:
   a. แจ้งผู้ใช้สั้นๆ ว่ากำลังส่งต่อให้ `requirement-writer` อัตโนมัติ
   b. เรียก agent `requirement-writer` พร้อมเนื้อหาใต้หัวข้อสัญญาณแบบ verbatim + วันที่ + บริบท
   c. รอผลลัพธ์ สุ่มตรวจ `backlog.md` อย่างน้อย 1 จุด
   d. เรียก agent `docs-maintainer` โหมด A ให้ `feature-list.md` สะท้อนรหัสใหม่ก่อน แล้วเรียก
      **Skill tool** ด้วย `skill: sync-architecture` ให้ architecture ครอบคลุมฟีเจอร์ใหม่ด้วย
      (เพราะ api-spec/db-spec ต้องอ้างอิง component จาก architecture เสมอ) แล้วจึงเรียก
      agent `api-db-writer` อีกรอบ
   e. **ป้องกัน infinite loop**: ทำซ้ำได้อีกไม่เกิน 1 รอบ

4. **ตรวจสอบผลลัพธ์ก่อนรายงาน**: สุ่มอ่าน `api-spec.md`/`db-spec.md` จริงอย่างน้อย 1-2 จุด ตรวจว่า
   field ระหว่างสองไฟล์ตรงกัน และไม่มี HTTP method/path, SQL type, หรือชื่อ technology ใดๆ หลุด
   เข้าไป

5. **สรุปให้ผู้ใช้ทราบ**: up to date หรือไม่ก่อนตรวจ, ส่วนที่แก้ไขในทั้งสองไฟล์, ความต้องการใหม่ที่
   auto-chain ไปให้ (ถ้ามี)

## ข้อควรระวัง

- ห้ามข้ามการเรียก agent แล้วเขียนไฟล์เองตรงๆ ในเทรดหลัก
- Agent ห้ามแตะไฟล์ใดๆ นอกจาก `api-spec.md`, `db-spec.md`, และไฟล์ log — หากผลลัพธ์พูดถึงการแก้ไข
  `architecture.md`/`feature-list.md`/`technology-stack.md` ให้หยุดและแจ้งผู้ใช้ทันที
- **ถ้าผลลัพธ์ระบุ HTTP method/path, protocol, SQL type, หรือชื่อ database engine ใดๆ ทั้งที่
  `technology-stack.md` ยังว่างเปล่า ให้ถือว่าผิดกฎ** หยุดและแจ้งผู้ใช้ทันที
- ห้ามวน auto-chain เกิน 1 รอบเด็ดขาด
- ถ้า agent รายงานว่า `architecture.md` ยังว่างเปล่า/ไม่ครอบคลุม ให้แจ้งผู้ใช้ให้รัน
  `sync-architecture` ก่อน
