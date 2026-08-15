---
name: sync-technical-spec
description: ตรวจสอบและสร้าง/ปรับปรุงเอกสารเชิงเทคนิคทั้งสายในคำสั่งเดียว (ไม่ผูก tech stack): docs/02-design/02-technical/architecture.md → api-spec.md/db-spec.md → detailed-design/{feature-slug}.md → nfr-review.md ให้สอดคล้องกับ docs/01-requirements/feature-list.md ล่าสุด ใช้เมื่อผู้ใช้ต้องการ "ทำ technical spec ทั้งหมด", "ทำ architecture ยัน detailed design", "sync เอกสารเทคนิคทั้งสาย" หรือพิมพ์ /sync-technical-spec
---

## เมื่อ skill นี้ถูกเรียก

Skill นี้ตรวจสอบความสอดคล้องของเอกสารเชิงเทคนิคทั้งสายตามลำดับ: `architecture.md` →
`api-spec.md`/`db-spec.md` → `detailed-design/` → `nfr-review.md` โดยทั้งหมดเขียนแบบ**ไม่ผูกกับ
tech stack** รันตามลำดับชั้นเสมอ **ไม่ใช่รันขนาน** เพราะแต่ละชั้นต้องใช้ผลลัพธ์ที่อัปเดตล่าสุดของ
ชั้นก่อนหน้าเป็นแหล่งความจริง ชั้น `nfr-review.md` เป็นชั้นเดียวที่**ตรวจสอบและรายงานเท่านั้น
ไม่ auto-fix**

1. **หาวันที่ปัจจุบัน**: รูปแบบ `YYYYMMDD` เพื่อส่งต่อให้ agent ทุกตัวใช้กับไฟล์ log

2. **ชั้นที่ 1 — architecture**: เรียกผ่าน **Skill tool** ด้วย `skill: sync-architecture` (มี
   auto-chain logic ไป `requirement-writer` ในตัวอยู่แล้ว) รอผลลัพธ์ก่อนไปต่อเสมอ

3. **ตรวจสอบผลลัพธ์ชั้นที่ 1**: สุ่มอ่าน `architecture.md` จริงอย่างน้อย 1 จุด ถ้าพบปัญหาเชิง
   โครงสร้างที่ทำให้ตรวจต่อไม่ได้ ให้หยุดทันทีและรายงานผู้ใช้

4. **ชั้นที่ 2 — API + DB spec**: เรียกผ่าน Skill tool ด้วย `skill: sync-api-db` **ต้องรอชั้นที่ 1
   เสร็จสมบูรณ์ก่อนเสมอ**

5. **ตรวจสอบผลลัพธ์ชั้นที่ 2**: สุ่มอ่าน `api-spec.md`/`db-spec.md` จริงอย่างน้อย 1 จุด

6. **ชั้นที่ 3 — Detailed Design**: เรียกผ่าน Skill tool ด้วย `skill: sync-detailed-design`
   **ต้องรอชั้นที่ 2 เสร็จสมบูรณ์ก่อนเสมอ**

7. **ตรวจสอบผลลัพธ์ชั้นที่ 3**: สุ่มอ่านไฟล์ใน `detailed-design/` จริงอย่างน้อย 1 จุด

8. **ชั้นที่ 4 — NFR Review (report-only)**: เรียก agent `nfr-reviewer` ผ่าน Agent tool
   (`run_in_background: false`) **ต้องรอชั้นที่ 3 เสร็จสมบูรณ์ก่อนเสมอ**

9. **ตรวจสอบผลลัพธ์ชั้นที่ 4**: ถ้าผลลัพธ์แสดงว่า `nfr-reviewer` แก้ไข/สร้างไฟล์ใดๆ นอกจาก
   `nfr-review.md`/log ให้ถือว่าผิดกฎ หยุดและแจ้งผู้ใช้ทันที

10. **สรุปผลรวมทั้งสายเป็นรายงานเดียว** แยกตามชั้น: architecture, api-spec/db-spec, detailed
    design, nfr-review (จำนวนประเด็นที่รองรับแล้ว/บางส่วน/ยังไม่รองรับ พร้อมคำแนะนำว่าควรรันชั้น
    ไหนซ้ำถ้ามีช่องว่าง)

## ข้อควรระวัง

- **ต้องรันทั้ง 4 ชั้นตามลำดับเสมอ ห้ามรันขนาน**
- ห้ามข้ามชั้นใดชั้นหนึ่งไปเอง แม้ผู้ใช้จะถามถึงแค่ปัญหาชั้นปลาย
- ห้ามข้ามการเรียก skill/agent แล้วแก้ไฟล์เองตรงๆ ในเทรดหลักไม่ว่าชั้นใด
- แต่ละ skill ย่อยมี loop guard auto-chain ของตัวเองอยู่แล้ว ไม่ต้องทำซ้ำในชั้นนี้
- ถ้า skill/agent ตัวใดรายงานปัญหาเชิงโครงสร้าง ให้หยุด pipeline ทันที ไม่ไปชั้นถัดไป แล้วรายงาน
  ผู้ใช้ก่อนเสมอ
- ทุกเอกสารในสายนี้ต้อง**ไม่ระบุชื่อ technology/framework/database engine ใดๆ** จนกว่า
  `technology-stack.md` จะถูกตัดสินใจแล้วจริง
