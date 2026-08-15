# 01 - Prototypes

เก็บ **ต้นแบบหน้าตาของระบบ (UI/UX Prototype)** เช่น

- Wireframe / mockup ของแต่ละหน้าจอ
- User flow และ navigation flow
- Design system เบื้องต้น เช่น สี ฟอนต์ คอมโพเนนต์หลัก

ใช้สำหรับสื่อสารและตกลงหน้าตาของระบบก่อนลงมือพัฒนาจริง โดยอ้างอิงความต้องการจาก [[../../01-requirements/01-spec/index|01-spec]] และส่งต่อรายละเอียดเชิงระบบให้ [[../02-technical/index|02-technical]]

## รายการ User Journey

รายการเอกสาร user journey ทั้งหมด เรียงจากล่าสุดไปเก่าสุด อ้างอิง spec ต้นทางใน [[../../01-requirements/01-spec/index|01-spec]] ไฟล์นี้ถูกอัปเดตอัตโนมัติโดย agent `docs-maintainer` (ดู `.claude/agents/docs-maintainer.md`) ทุกครั้งที่มีการสร้าง/แก้ไข user journey

| วันที่ | Actor | หัวข้อ | เอกสาร | อ้างอิงจาก spec |
|---|---|---|---|---|
| 2026-08-15 | เจ้าของร้าน/ผู้บริหาร (owner) | ดู Sales Dashboard | [[20260815-03-user-journey-owner-sales-dashboard|20260815-03-user-journey-owner-sales-dashboard]] | [[../../01-requirements/01-spec/20260802-02-sales-dashboard|20260802-02-sales-dashboard]] |
| 2026-08-15 | ลูกค้า (customer) | สั่งกาแฟผ่าน QR ที่โต๊ะและชำระเงิน (รวม PDPA consent) | [[20260815-02-user-journey-customer-qr-order-payment|20260815-02-user-journey-customer-qr-order-payment]] | [[../../01-requirements/01-spec/20260802-01-table-self-order-qr|20260802-01-table-self-order-qr]] |
| 2026-08-15 | พนักงาน (staff) | การขายหน้าร้านผ่าน POS | [[20260815-01-user-journey-staff-pos-checkout|20260815-01-user-journey-staff-pos-checkout]] | [[../../01-requirements/01-spec/20260815-01-pos-point-of-sale|20260815-01-pos-point-of-sale]] |

