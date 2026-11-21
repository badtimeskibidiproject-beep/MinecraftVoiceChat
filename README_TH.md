# Minecraft Voice Chat Android — Prototype

โปรเจกต์ Android ต้นแบบหน้าตา Minecraft Original UI สำหรับต่อยอดระบบ voice chat

## ข้อเท็จจริงสำคัญ
- โค้ดนี้เป็น **ต้นแบบ UI + ขอสิทธิ์ไมโครโฟน + โครง WebSocket signaling**
- **ยังไม่ใช่แอป Voice Chat ที่ใช้งานเสียงจริง** เพราะยังไม่ได้รวม WebRTC audio engine, การจัดการ peer connections, ICE/STUN/TURN และ SFU/การส่งเสียงแบบหลายคน
- ปุ่มไมค์ในต้นแบบเปลี่ยนสถานะ/ส่งข้อความ mute เท่านั้น ไม่ได้เปิด audio capture pipeline
- ต้องมี Voice Server ที่เปิดใช้งานผ่าน WSS และมีการยืนยันตัวตนจริงก่อนใช้งานนอกเครือข่าย
- ห้ามใส่ token จริงไว้ในแอปที่แจกจ่ายสาธารณะ เพราะสามารถถูกดึงจาก APK ได้

## วิธี Build APK โดยมีแค่โทรศัพท์ Android
ใช้ GitHub Actions ผ่านเว็บเบราว์เซอร์ในโทรศัพท์:
1. สร้างบัญชี GitHub หากยังไม่มี
2. เปิด GitHub แล้วสร้าง repository ใหม่ชื่อ `MinecraftVoiceChat`
3. อัปโหลดไฟล์ทั้งหมดใน ZIP นี้เข้า repository โดยคงโครงสร้างโฟลเดอร์
4. เปิดแท็บ **Actions** ของ repository
5. เลือก workflow **Build Android APK** แล้วกด **Run workflow**
6. เมื่อ workflow สำเร็จ เปิดรายการ run ล่าสุด แล้วดาวน์โหลด artifact `MinecraftVoiceChat-debug`
7. แตก ZIP ที่ดาวน์โหลด จะพบ `app-debug.apk`
8. ติดตั้ง APK บนโทรศัพท์ โดย Android อาจขออนุญาตติดตั้งจากแหล่งที่มานี้

## ตั้งค่า server
แก้ `app/src/main/java/com/example/minecraftvoicechat/MainActivity.kt`:
- `serverUrl`: ต้องเป็น URL `wss://` ของเซิร์ฟเวอร์ที่ deploy แล้ว
- `roomId`: รหัสห้องของ BDS
- `accessToken`: token สำหรับทดสอบส่วนตัวเท่านั้น

อย่าใช้ `ws://` ผ่านอินเทอร์เน็ตสาธารณะ; ใช้ `wss://` พร้อม TLS

## หมายเหตุ
ไฟล์นี้ยังไม่รวม Gradle wrapper และไม่สามารถรับประกันว่า build ผ่านทุก environment โดยไม่ทดสอบ dependency/SDK versions
