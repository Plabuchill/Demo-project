# CLAUDE.md

ไฟล์นี้ให้คำแนะนำแก่ Claude Code (claude.ai/code) เมื่อทำงานกับโค้ดในโปรเจกต์นี้

## สถานะของโปรเจกต์

โปรเจกต์นี้ในปัจจุบัน **มีเพียงโครงเอกสาร (documentation scaffold)** อยู่ใต้ `docs/` เท่านั้น — ยังไม่มีซอร์สโค้ดของแอปพลิเคชัน, package manifest, เครื่องมือ build, linter หรือชุดทดสอบใด ๆ อย่าสันนิษฐานว่ามี tech stack, framework หรือคำสั่งอย่าง `npm install`/`npm test` อยู่ ให้ตรวจสอบด้วย `Glob`/`Bash` ก่อนแนะนำหรือรันคำสั่ง build/lint/test ใด ๆ เมื่อมีการเพิ่มซอร์สโค้ดในอนาคต ควรอัปเดตไฟล์นี้ให้มีคำสั่งและสถาปัตยกรรมที่ตรงกับความจริง

## โครงสร้างเอกสาร

เอกสารอยู่ใน `docs/` ในรูปแบบฐานความรู้สไตล์ Obsidian เขียนเป็นภาษาไทยเป็นหลัก แต่ละโฟลเดอร์มีไฟล์ `index.md` อธิบายจุดประสงค์และลิงก์ไปยังโฟลเดอร์ที่เกี่ยวข้องด้วย wiki-link (`[[../path/index|label]]`) โฟลเดอร์เหล่านี้สะท้อนขั้นตอนการทำงานของโปรเจกต์เป็นลำดับขั้น (pipeline) ที่มีหมายเลขกำกับ — ผลลัพธ์ของแต่ละขั้นจะถูกส่งต่อไปยังขั้นถัดไป:

```
01-requirements/01-spec   → ระบบต้องทำอะไรบ้าง (ต้นทางของความต้องการ)
01-requirements/02-plan   → roadmap/timeline ที่แตกมาจาก spec
01-requirements/03-task   → งานย่อยที่แตกมาจากแผนงาน
02-design/01-prototypes   → mockup ของ UI/UX อ้างอิงจาก 01-spec
02-design/02-technical    → architecture, DB schema, API design — พิมพ์เขียวสำหรับการพัฒนา
03-testing/01-test-plan   → test case ที่แตกมาจาก 01-spec และ 02-design
03-testing/02-test-result → ผล pass/fail จริงและบั๊กที่พบ
04-retrospectives         → บทเรียนที่ได้ในแต่ละ phase/sprint อ้างอิงจาก 02-test-result และ 05-log
05-log                    → changelog และบันทึกการตัดสินใจตามลำดับเวลา
00-archived               → เอกสารที่เลิกใช้แล้ว/ถูกยกเลิก — ห้ามลบเอกสาร ให้ย้ายมาไว้ที่นี่แทน
```

เมื่อจะเพิ่มหรือแก้ไขเอกสาร ให้วางไว้ในโฟลเดอร์ที่ตรงกับขั้นตอนใน pipeline นี้ ให้ปฏิบัติตามธรรมเนียม wiki-link ของ `index.md` ที่มีอยู่แล้วเมื่ออ้างอิงข้ามไปยังส่วนอื่น และเขียนเป็นภาษาไทยให้สอดคล้องกับเนื้อหาที่มีอยู่ เว้นแต่จะได้รับแจ้งเป็นอย่างอื่น

**ข้อสำคัญ**: โปรเจกต์นี้**ไม่ใช้รหัส FR-xx/NFR-xx** ทุกที่ — `backlog.md` เป็นตารางระดับเอกสาร (1 แถวต่อ 1 ไฟล์ spec) และ `feature-list.md` แตกเป็นระดับฟีเจอร์พร้อม MoSCoW แบบ Must/Won't เท่านั้น (Should/Could เป็นหน้าที่ของ `02-plan/` ในภายหลัง) ทุก agent/skill ในโปรเจกต์นี้อ้างอิงฟีเจอร์ด้วยข้อความ + wikilink กลับไปยัง spec ต้นทาง ไม่ใช่ด้วยรหัส

## เครื่องมืออัตโนมัติดูแลความสอดคล้องของเอกสาร (agents & skills)

โปรเจกต์นี้มี custom agents ใน `.claude/agents/` และ skills ใน `.claude/skills/` สำหรับสร้าง/ตรวจสอบความสอดคล้องของเอกสารแต่ละชั้นให้ตรงกับชั้นก่อนหน้าเสมอ ตามลำดับ: spec → `backlog.md`/`feature-list.md` → user journey (`01-prototypes/*.md`) → แตกแขนงขนานกัน 3 สาย (technical spec ใน `02-technical/`, test plan ใน `03-testing/01-test-plan/`, HTML prototype ใน `01-prototypes/{version}/`) → phase plan ใน `02-plan/`+`03-task/` เมื่อผู้ใช้ขอให้ทำงานที่ตรงกับหน้าที่ของ skill ใดอยู่แล้ว **ให้เรียกใช้ skill/agent นั้นแทนการแก้ไฟล์เอกสารตรงๆ เอง** เพื่อให้การตรวจสอบ cross-file consistency และการบันทึกสรุปงานลง `docs/05-log/{YYYYMMDD}-log.md` เป็นไปตามรูปแบบเดิมของโปรเจกต์

จุดเริ่มต้นที่ใช้บ่อย:
- `/create-requirement` — แปลง requirement ดิบจากผู้ใช้เป็นเอกสาร spec ใหม่/แก้ไขของเดิม พร้อมอัปเดต backlog (agent: `requirement-writer`)
- `/audit-feature-list` — ตรวจ spec ↔ backlog ↔ สร้าง/อัปเดต `feature-list.md` พร้อม MoSCoW (agent: `docs-maintainer` โหมด A)
- `/create-user-journey` — สร้าง/อัปเดตเอกสาร user journey ต่อ actor พร้อม Mermaid flowchart (agent: `docs-maintainer` โหมด B)
- `/sync-architecture`, `/sync-api-db`, `/sync-detailed-design` (รวมเป็น `/sync-technical-spec` ได้), `/sync-test-plan`, `/sync-phase-plan`, `/build-prototype` — สร้าง/sync เอกสารแต่ละชั้นให้ตรงกับชั้นก่อนหน้า (agents: `architecture-writer`, `api-db-writer`, `detailed-design-writer`, `nfr-reviewer` (report-only), `test-writer`, `phase-planner`, `prototype-writer`)
- `/run-requirements-phase`, `/run-technical-phase`, `/run-prototype-phase` — รวมหลายขั้นตอนที่เกี่ยวข้องกันไว้ในคำสั่งเดียว
- `/audit-pipeline` — ตรวจสอบความสอดคล้องทั้งสายงานตั้งแต่ spec ถึงปลายทางในคำสั่งเดียว

เอกสารเชิงเทคนิคทั้งหมด (`architecture.md`, `api-spec.md`, `db-spec.md`, `detailed-design/`) เขียนแบบไม่ผูก tech stack จนกว่า `docs/02-design/02-technical/technology-stack.md` จะถูกตัดสินใจแล้วจริง — ห้าม assume framework/database engine ใดๆ ก่อนหน้านั้น ส่วน `prototype-writer`/`build-prototype` ต้องมี `docs/02-design/DESIGN.md` (Design System) ก่อนเสมอ ซึ่ง**ยังไม่มี agent ใดสร้างให้อัตโนมัติ** — ต้องให้ผู้ใช้สร้าง/เติมเอง

## ธรรมเนียมการใช้ Git

- Remote `origin` คือ `https://github.com/Plabuchill/Demo-project.git`
- `.claude/settings.local.json` ปัจจุบัน allow-list คำสั่ง `git commit`/`git push` ไว้แล้ว — ให้ถือว่ายังคงต้องขอคำยืนยันตามหลักเดียวกับการ push/publish อื่น ๆ ที่ระบุไว้ในคำแนะนำการทำงานของคุณ ไม่ใช่การอนุญาตล่วงหน้าแบบครอบคลุมทุกกรณี
