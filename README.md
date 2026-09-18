# SmartCanteen+ — Public Demo

SmartCanteen+ คือเว็บแอปโรงอาหารโรงเรียนแบบ Mobile-first สำหรับสั่งอาหารล่วงหน้า จัดการเครดิต โอนเครดิตระหว่างนักเรียน และติดตามข้อมูลผ่านหน้าจอครูและร้านค้า

## ทดลองใช้งาน

- [Vercel](https://smartcanteen-plus.vercel.app)
- [GitHub Pages](https://pulawatchapibansri.github.io/SmartCanteen-Plus-Demo/)
- [อ่าน Portfolio Case Study ฉบับเต็ม](docs/PORTFOLIO.md)

## บัญชีเดโม

| บทบาท | ชื่อผู้ใช้ | รหัสผ่าน |
| --- | --- | --- |
| ครูผู้ดูแล | `teacher` | `Teacher@2026` |
| ร้าน 1–5 | `shop01`–`shop05` | `Shop@2026` |
| นักเรียน | Google Sign-In | ใช้บัญชี Google |

## ความสามารถที่สาธิต

- นักเรียน: Google Login, กระเป๋าเครดิต, PromptPay QR, โอนเครดิต, สั่งอาหาร และติดตามออเดอร์
- ร้านค้า: จัดการคิว สถานะออเดอร์ เมนู รูปภาพ หมวด และยอดขาย
- ครู: สถิติ ห้องเรียน รายละเอียดนักเรียน ร้านค้า และวงเงินธุรกรรม
- Supabase Auth, PostgreSQL, RLS และ atomic RPC สำหรับ student flow
- Responsive UI สำหรับโทรศัพท์ พร้อม 39 automated tests

## ขอบเขต Prototype

- PromptPay QR ผูกจำนวนเงินได้ แต่ยังไม่ตรวจสอบเงินเข้าหรือสลิปอัตโนมัติ
- เครดิตเป็นเครดิตเดโม ไม่สามารถถอนเป็นเงินจริง
- บัญชีครูและร้านค้าเป็น demo accounts ใน browser
- ไม่มี ESP32 หรือ RFID ในเวอร์ชันนี้

Repository นี้เก็บ production build และข้อมูลสรุปสำหรับการนำเสนอ ส่วน source repository หลักเก็บเป็น private เพื่อรักษาการตั้งค่าและ workflow ของโครงการ

## ผู้พัฒนา

ปุลวัชร อภิบาลศรี — Product concept, UX/UI, system architecture, database และ prototype development
