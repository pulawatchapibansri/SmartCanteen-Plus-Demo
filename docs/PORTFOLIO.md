# SmartCanteen+ — Portfolio Case Study

## ภาพรวม

SmartCanteen+ คือ functional prototype ของระบบโรงอาหารอัจฉริยะที่ออกแบบเพื่อแก้ 2 ปัญหาหลักของโรงเรียน: การเติมบัตรที่ต้องใช้เงินสด และคิวซื้ออาหารที่หนาแน่นในช่วงพัก ผู้ใช้เข้าผ่านเว็บบนโทรศัพท์ โดยระบบแยกประสบการณ์ของนักเรียน ร้านค้า และครูผู้ดูแลอย่างชัดเจน

## บทบาทของผู้พัฒนา

ปุลวัชร อภิบาลศรีรับผิดชอบการกำหนดปัญหาและขอบเขต ออกแบบ UX/UI และ architecture ออกแบบฐานข้อมูล พัฒนา frontend เชื่อม Supabase วางกลไกเครดิตและออเดอร์ เขียน automated tests และนำระบบขึ้น GitHub Pages กับ Vercel

## กระบวนการพัฒนา

### 1. ลดขอบเขตให้เป็น MVP ที่ทดสอบได้

แนวคิดแรกมีทั้ง RFID, ESP32, blockchain และการชำระเงินอัตโนมัติ แต่เมื่อต้องทำต้นแบบภายในเวลาจำกัด จึงตัด hardware และการยืนยันเงินจากธนาคารออก แล้วเน้น flow ที่สาธิตคุณค่าหลักได้จริง ได้แก่ สั่งอาหารล่วงหน้า กระเป๋าเครดิต โอนเครดิต หน้าคิวร้านค้า และแดชบอร์ดครู

### 2. ออกแบบข้อมูลและสิทธิ์

ระบบแบ่งข้อมูลเป็น profiles, classrooms, shops, menus, wallets, transactions, orders และ order items ใช้ Supabase Auth สำหรับนักเรียน และเตรียม Row Level Security เพื่อไม่ให้ผู้ใช้เข้าถึงข้อมูลที่ไม่เกี่ยวข้อง ธุรกรรมสำคัญเรียกผ่าน database functions เพื่อให้การหักยอดและบันทึก ledger เกิดพร้อมกัน

### 3. ออกแบบ UX ตามบทบาท

- นักเรียนต้องเห็นยอดเงิน การสั่งอาหาร และการโอนอย่างรวดเร็วบนมือถือ
- ร้านค้าต้องจัดการออเดอร์ด้วยปุ่มใหญ่ เห็นสถานะชัด และแก้เมนูได้เอง
- ครูต้องดูภาพรวม แล้วเจาะจากห้องไปยังนักเรียนหรือจากร้านไปยังยอดขายได้

Visual direction ใช้สีเขียวเข้ม พื้นหลังอ่อน และ typography ที่อ่านง่าย เพื่อให้คล้ายผลิตภัณฑ์การเงินที่น่าเชื่อถือมากกว่า dashboard สำเร็จรูป

### 4. ทดสอบจากปัญหาจริง

นอกจาก unit และ integration tests ยังทดสอบบน deployment จริงหลายบทบาท แล้วเปลี่ยนบั๊กที่พบให้เป็น regression tests เช่น login staff หลังใช้ Google, การผูกบัญชีร้านกับ UUID จริง และช่องตัวเลขที่ลบค่าให้ว่างไม่ได้

## ปัญหาเทคนิคที่แก้ระหว่างทาง

### GitHub Pages แสดงหน้าขาว

Vite build เดิมอ้าง asset จาก root แต่ GitHub Pages เปิดเว็บใต้ subpath จึงแก้ build base ให้ตรงกับ `/SmartCanteen-Plus-Demo/` ขณะที่ Vercel ใช้ `/` จากนั้นตรวจหน้าเว็บจริงทั้งสอง host

### บัญชีครูและร้านเข้าไม่ได้หลัง Google Login

พบว่า Supabase session เดิมอาจทำให้แอปข้ามฟอร์ม staff และข้อมูลร้านในฐานข้อมูลใช้ UUID ต่างจากข้อมูลเดโม จึงแยก staff login flow ให้ชัดและ map ร้านตาม account slug แทนการยึด ID เดโม

### ช่องจำนวนเงินบังคับกลับเป็นศูนย์

controlled number input แปลงค่าว่างเป็น `0` ทันที ทำให้ผู้ใช้ลบเลขเดิมไม่ได้ จึงเก็บค่าระหว่างพิมพ์เป็น string และ validate ตอน submit พร้อมเพิ่ม test ป้องกันบั๊กย้อนกลับ

## ผลลัพธ์

- เว็บ responsive ที่ใช้งานผ่านโทรศัพท์และเดสก์ท็อป
- สาม user roles พร้อมหน้าจอและสิทธิ์ต่างกัน
- Google OAuth ผ่าน Supabase สำหรับนักเรียน
- PromptPay QR ที่ผูกจำนวนเงินตามยอดที่กรอก
- ระบบเครดิต โอน ออเดอร์ ร้านค้า เมนู ห้องเรียน และสถิติ
- 39 automated tests พร้อม typecheck และ production build
- เผยแพร่บน Vercel และ GitHub Pages

## ขอบเขตที่สื่อสารอย่างโปร่งใส

PromptPay QR สามารถแสดงยอดและบัญชีผู้รับ แต่ระบบยังไม่ยืนยันว่าเงินเข้าจริง ปุ่มเพิ่มเครดิตเดโมจึงแยกจากการชำระเงินจริง บัญชี staff และรูปเมนูบางส่วนยังเก็บใน browser ส่วน Supabase schema และ security model เป็นฐานสำหรับพัฒนาระบบ production ต่อไป

## ทักษะที่แสดงผ่านโครงการ

- Product scoping และการตัดสินใจเชิง MVP
- Responsive UX/UI และ role-based product design
- React, TypeScript และ state management
- Relational database, RLS, ledger และ atomic transactions
- OAuth integration และ deployment configuration
- Testing, debugging และ regression prevention
- การอธิบายข้อจำกัดของ prototype อย่างรับผิดชอบ

## ลิงก์

- [ทดลองบน Vercel](https://smartcanteen-plus.vercel.app)
- [ทดลองบน GitHub Pages](https://pulawatchapibansri.github.io/SmartCanteen-Plus-Demo/)
- [Public demo repository](https://github.com/pulawatchapibansri/SmartCanteen-Plus-Demo)
