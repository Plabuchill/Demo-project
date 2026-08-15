---
name: sync-test-plan
description: ตรวจสอบและสร้าง/ปรับปรุงเอกสารทดสอบ 3 ส่วนใน docs/03-testing/01-test-plan/ (acceptance-criteria.md, test-plan.md, test-cases/{feature-slug}.md) ให้สอดคล้องกับ docs/01-requirements/feature-list.md และเอกสาร user journey ล่าสุด ใช้เมื่อผู้ใช้ต้องการ "สร้าง test plan", "ทำ test case", "เขียน acceptance criteria", "อัปเดต test plan จาก feature list" หรือพิมพ์ /sync-test-plan
---

## เมื่อ skill นี้ถูกเรียก

1. มอบหมายให้ agent `test-writer` ผ่าน Agent tool (`run_in_background: false` เพราะอาจต้องถาม
   ผู้ใช้แบบโต้ตอบกรณีเจอไฟล์ test case ล้าสมัย) พร้อมวันที่ปัจจุบันและบริบท session สนทนาจริง

2. **ตรวจจับสัญญาณ "ต้องการ requirement ใหม่"**: ไม่มีหัวข้อ `## NEEDS_NEW_REQUIREMENT` → ข้ามไป
   ข้อ 4 มีหัวข้อนี้ → ทำข้อ 3 ก่อน

3. **Auto-chain ไป `requirement-writer` (ไม่เกิน 1 รอบ)**:
   a. แจ้งผู้ใช้สั้นๆ ว่าเจอความต้องการใหม่ระหว่างเขียน acceptance criteria กำลังส่งต่อให้
      `requirement-writer` อัตโนมัติ
   b. เรียก agent `requirement-writer` พร้อมเนื้อหาใต้หัวข้อสัญญาณแบบ verbatim + วันที่ + บริบท
   c. รอผลลัพธ์ สุ่มตรวจ `backlog.md` อย่างน้อย 1 จุด
   d. เรียก agent `docs-maintainer` โหมด A ให้ `feature-list.md` สะท้อนฟีเจอร์ใหม่ก่อน แล้วเรียก
      agent `test-writer` อีกรอบ (prompt เดิม)
   e. **ป้องกัน infinite loop**: ทำซ้ำได้อีกไม่เกิน 1 รอบ

4. **ตรวจสอบผลลัพธ์ก่อนรายงาน**: สุ่มอ่าน `acceptance-criteria.md`, `test-plan.md`, และไฟล์ใน
   `test-cases/` จริงอย่างน้อย 1-2 จุด ตรวจว่า AC/test case แต่ละข้ออ้างอิงฟีเจอร์จริงครบ

5. **สรุปให้ผู้ใช้ทราบ**: up to date อยู่แล้วหรือไม่ก่อนตรวจ, ไฟล์ที่สร้าง/แก้ไข, ความต้องการใหม่ที่
   auto-chain ไปให้ (ถ้ามี), จุดที่ถาม/รอผู้ใช้ตัดสินใจ (ถ้ามี)

## ข้อควรระวัง

- ห้ามข้ามการเรียก agent แล้วเขียนเอกสารทดสอบเองตรงๆ ในเทรดหลัก
- Agent ห้ามแตะไฟล์ใดๆ นอกจากในโฟลเดอร์ `01-test-plan/` กับไฟล์ log — หากผลลัพธ์พูดถึงการแก้ไข
  `feature-list.md`, เอกสาร journey, หรือไฟล์ใน `01-spec/`/`02-test-result/` ให้หยุดและแจ้งผู้ใช้ทันที
- ห้ามวน auto-chain เกิน 1 รอบเด็ดขาด
- ถ้า agent รายงานว่า `feature-list.md` ว่างเปล่า/ไม่มี ให้แจ้งผู้ใช้ให้รัน `audit-feature-list` ก่อน
