---
name: sync-detailed-design
description: ตรวจสอบและสร้าง/ปรับปรุงไฟล์ใน docs/02-design/02-technical/detailed-design/{feature-slug}.md (sequence flow, state transition, edge case ต่อฟีเจอร์ แบบไม่ผูก tech stack) ให้สอดคล้องกับ api-spec.md/db-spec.md ล่าสุด ใช้เมื่อผู้ใช้ต้องการ "ทำ detailed design", "ออกแบบ component ของฟีเจอร์นี้", "อัปเดต detailed design จาก api spec" หรือพิมพ์ /sync-detailed-design
---

## เมื่อ skill นี้ถูกเรียก

1. มอบหมายให้ agent `detailed-design-writer` ผ่าน Agent tool (`run_in_background: false` เพราะ
   อาจต้องถามผู้ใช้แบบโต้ตอบผ่าน AskUserQuestion กรณีเจอไฟล์ล้าสมัย) พร้อมวันที่ปัจจุบันและบริบท
   session สนทนาจริง

2. **รอผลลัพธ์**: ปล่อยให้ agent จัดการคำถามผู้ใช้เอง อย่าตอบแทน

3. **ตรวจสอบผลลัพธ์ก่อนรายงาน**: สุ่มอ่านไฟล์ใน `detailed-design/` จริงอย่างน้อย 1-2 จุด ตรวจว่า
   operation/entity ที่อ้างถึงมีอยู่จริงใน `api-spec.md`/`db-spec.md` และไม่มี framework/library
   เฉพาะภาษาใดๆ หลุดเข้าไป

4. **สรุปให้ผู้ใช้ทราบ**: up to date หรือไม่ก่อนตรวจ, ไฟล์ที่สร้าง/แก้ไข, จุดที่ถามผู้ใช้ (ถ้ามี),
   ช่องว่างของ api-spec/db-spec ที่ agent รายงาน (ถ้ามี)

## ข้อควรระวัง

- ห้ามข้ามการเรียก agent แล้วเขียนไฟล์เองตรงๆ ในเทรดหลัก
- Agent ห้ามแตะไฟล์ใดๆ นอกจากไฟล์ใน `detailed-design/` กับไฟล์ log — หากผลลัพธ์พูดถึงการแก้ไข
  `api-spec.md`/`db-spec.md`/`architecture.md`/`feature-list.md` ให้หยุดและแจ้งผู้ใช้ทันที
- **ถ้าผลลัพธ์ระบุ framework/library/pattern เฉพาะภาษาใดๆ ทั้งที่ `technology-stack.md` ยังว่าง
  เปล่า ให้ถือว่าผิดกฎ** หยุดและแจ้งผู้ใช้ทันที
- ถ้า agent รายงานว่า `api-spec.md`/`db-spec.md` ยังไม่ครอบคลุมฟีเจอร์ที่ต้องออกแบบ ให้แจ้งผู้ใช้ให้
  รัน `sync-api-db` ก่อน
- ไม่มี auto-chain ไป `requirement-writer` ในชั้นนี้ (agent นี้ถูกจำกัดให้ใช้ operation/entity ที่มี
  อยู่แล้วเท่านั้น ถ้าขาดให้แนะนำ `sync-api-db` แทนเสมอ)
