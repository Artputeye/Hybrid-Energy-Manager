# Hybrid Energy Manager (HEM) — Project Development History

> บันทึกเส้นทางการพัฒนาโปรเจกต์ตั้งแต่เริ่มต้นจนถึงสถานะปัจจุบัน ใช้ย้อนดูว่าระบบเริ่มจากอะไร พัฒนาเพิ่มส่วนใด แก้ปัญหาอะไร และใช้เป็นลำดับอ้างอิงเมื่อต้องสร้างโปรเจกต์ใหม่  
> Project root: `D:\@Project\Hybrid-Energy-Manager`  
> Repository: https://github.com/Artputeye/Hybrid-Energy-Manager  
> Development period verified from Git history: 2026-07-11 – 2026-11-04

---

## 1. ภาพรวมการเดินทางของโปรเจกต์

โปรเจกต์เริ่มจาก ESP32 Controller สำหรับ Hybrid Inverter โดยมีเป้าหมายหลักในการอ่านสถานะและควบคุม Inverter ผ่าน Serial/UART จากนั้นค่อย ๆ เพิ่มระบบเครือข่าย, Energy Tracking, MQTT/Home Assistant, Web Dashboard, การคำนวณค่าไฟ, Energy History, Fault Monitoring และการตั้งค่า Battery

ลำดับการเติบโต:
1. เริ่มจาก Firmware และการสื่อสารกับ Inverter — วางโครงสร้าง ESP32/PlatformIO และโค้ดควบคุมพื้นฐาน
2. เพิ่ม Network — แก้ Wi-Fi reconnect และ AP mode
3. เพิ่ม Energy Tracking — Energy Tracker และ uptime
4. จัดระเบียบโค้ดและ Service — แยก inverter module, route management และ MQTT
5. เพิ่ม Grid/PV และข้อมูลพลังงาน — Grid Power, PV limit, Energy History และ Fault status
6. พัฒนา Web UI และค่าไฟ — Dashboard, settings, progressive tariff และ summaries
7. ปรับความถูกต้องของข้อมูล — Monthly range/reset และ Battery 24V/48V
8. ยกระดับเป็น Hybrid Energy Manager — เปลี่ยนชื่อและจัดทำเอกสารอ้างอิง

> โปรเจกต์นี้พัฒนาแบบเพิ่มความสามารถจากระบบที่ใช้งานได้ ไม่ใช่เริ่มจากการออกแบบระบบใหม่ทั้งหมด เอกสารนี้จึงบันทึกวิวัฒนาการของของเดิม ไม่ได้กำหนดให้ refactor หรือเปลี่ยนสถาปัตยกรรมโดยไม่มีเหตุผล

## 2. Timeline จาก Git History

วันที่และ commit อ้างอิงจาก Git log ใน repository เรียงจากเก่าไปใหม่ ชื่อ commit ใช้เป็นหลักฐานของ milestone ส่วนรายละเอียดที่ไม่ได้ระบุใน commit เป็นคำอธิบายระดับภาพรวม

| วันที่ | Commit | Milestone / สิ่งที่พัฒนา |
|---|---|---|
| 2026-07-11 | `0d4b6ff` | First commit — เริ่มต้น repository |
| 2026-07-11 | `be05a28` | First code — เพิ่มโค้ดชุดแรก |
| 2026-07-13 | `564b694` | Fix Wi-Fi reconnect — ปรับการเชื่อมต่อ Wi-Fi ใหม่เมื่อหลุด |
| 2026-07-13 | `d13623a` | Add Energy Tracker — เริ่มเพิ่มระบบติดตามพลังงาน |
| 2026-07-15 | `bf5cabe` | Fix Wi-Fi AP mode can't connect — แก้ปัญหาเชื่อมต่อ AP mode ไม่ได้ |
| 2026-07-16 | `2031dff` | Fix uptime sec to day — ปรับ uptime จากวินาทีเป็นวัน |
| 2026-07-17 | `c0df1ff` | Change `inv_command` to `invHybrid` — ปรับชื่อโมดูล Inverter |
| 2026-07-18 | `2d8a11c` | Add function manage route path — เพิ่ม/จัดการ HTTP route |
| 2026-07-21 | `7204879` | เปลี่ยนแนวทาง MQTT library จาก ArduinoHA header ไปใช้ PubSubClient |
| 2026-08-04 | `75e06d5` | Fix grid power, add PV limit current — ปรับ Grid Power และเพิ่ม PV limit current |
| 2026-08-08 | `95fd505` | Improve UI for modern style — ปรับ Web UI ให้ทันสมัย |
| 2026-09-28 | `42c1527` | Refresh dashboard and settings pages — ปรับ Dashboard และ Settings |
| 2026-09-30 | `ae630da` | Update expense settings UI, progressive electricity rate calculation, and dashboard flow — เพิ่ม UI ค่าไฟ อัตราค่าไฟแบบขั้นบันได และ Dashboard flow |
| 2026-10-01 | `56a5791` | Add energy cost and dashboard summaries — เพิ่ม Energy Cost และ Dashboard Summary |
| 2026-10-02 | `d4ed726` | Add energy history and dashboard status indicators — เพิ่ม Energy History และตัวบ่งชี้สถานะ |
| 2026-10-02 | `cb62d8f` | Link `faultList` via WebSocket to dashboard status-item and state fault row — เชื่อม Fault list ไป Dashboard และ State |
| 2026-10-04 | `21ff5b4` | Sync hybrid inverter project updates — รวม/ซิงก์การเปลี่ยนแปลง |
| 2026-10-04 | `16600cd` | Improve README presentation — ปรับ README |
| 2026-10-30 | `9ae4228` | Fix 24V/48V battery voltage settings — แก้การตั้งค่าแรงดัน Battery สำหรับ 24V/48V |
| 2026-11-03 | `d08aea6` | Fix monthly reset for cross-month ranges — แก้ monthly reset เมื่อช่วงวันที่คร่อมเดือน |
| 2026-11-04 | `b5b1167` | Rename project to Hybrid Energy Manager — เปลี่ยนชื่อโปรเจกต์และเอกสารเป็น HEM |

## 3. พัฒนาการแยกตามช่วงเวลา

### Stage 1 — เริ่มต้น ESP32 และ Inverter (11–12 กรกฎาคม 2026)
- สร้าง Git repository และเพิ่ม source code ชุดแรก
- วางพื้นฐาน ESP32 + PlatformIO + Arduino
- ใช้ ESP32 เป็น Controller เชื่อมต่อ Hybrid Inverter
- การสื่อสารใช้ Serial2/UART; โปรเจกต์ปัจจุบันระบุ 2400 baud, 8N1
- วางพื้นฐานการอ่านค่าการทำงานและส่งคำสั่งไปยัง Inverter

**ผลลัพธ์:** มี repository และ firmware ตั้งต้นสำหรับพัฒนาฟีเจอร์ต่อ

### Stage 2 — ทำให้ Network ใช้งานต่อเนื่อง (13–16 กรกฎาคม 2026)
- แก้ Wi-Fi reconnect เมื่อการเชื่อมต่อหลุด
- เริ่มเพิ่ม Energy Tracker
- แก้ AP mode ที่อุปกรณ์อื่นเชื่อมต่อไม่ได้
- ปรับ uptime ให้อ่านเป็นหน่วยวัน

**บทเรียน:** Network reconnect ต้องไม่ทำให้การอ่าน Inverter หรือระบบหลักหยุดชะงัก

### Stage 3 — จัดระเบียบ Inverter และ Service Layer (17–21 กรกฎาคม 2026)
- เปลี่ยนชื่อโมดูลจาก `inv_command` เป็น `invHybrid`
- เพิ่มการจัดการ HTTP route path
- เปลี่ยนแนวทาง MQTT library จาก ArduinoHA header ไปใช้ PubSubClient
- เริ่มแยก Inverter communication, Web route และ MQTT ตามหน้าที่

**บทเรียน:** เมื่อฟังก์ชันเพิ่มขึ้น ควรแยกโมดูลตามหน้าที่แทนการรวมทุกอย่างไว้ในไฟล์หลัก

### Stage 4 — เพิ่มข้อมูล Grid และ Solar (4 สิงหาคม 2026)
- ปรับปรุง Grid Power
- เพิ่มการจัดการ PV limit current
- ขยายระบบจากการอ่านค่า Inverter ไปสู่ข้อมูลสำหรับติดตามและควบคุมพลังงาน

### Stage 5 — ยกระดับ Web UI (8 สิงหาคม – 28 กันยายน 2026)
- ปรับ UI ให้ทันสมัย
- Refresh หน้า Dashboard และ Settings
- แยกหน้าและไฟล์ frontend ให้รองรับการขยายระบบ
- Dashboard เป็นจุดรวมข้อมูลจาก Inverter และระบบ

### Stage 6 — ระบบค่าไฟและ Dashboard Analytics (30 กันยายน – 1 ตุลาคม 2026)
- ปรับหน้า Expense Settings
- เพิ่มการคำนวณค่าไฟแบบ progressive/tier rate
- ปรับ Dashboard flow
- เพิ่ม Energy Cost และ Dashboard Summary
- เชื่อมข้อมูลพลังงานกับค่าใช้จ่ายและผลประหยัด

**ผลลัพธ์:** ระบบเริ่มสรุปข้อมูลเป็นค่าใช้พลังงานและภาพรวมที่เข้าใจได้ ไม่ได้แสดงเพียงค่ากำลังไฟ

### Stage 7 — Energy History และ Fault Monitoring (2–4 ตุลาคม 2026)
- เพิ่ม Energy History
- เพิ่ม Dashboard status indicators
- เชื่อม `faultList` ผ่าน WebSocket
- แสดง Fault ใน Dashboard และ State page
- Sync การเปลี่ยนแปลงและปรับ README

**ผลลัพธ์:** ผู้ใช้เห็นข้อมูลปัจจุบัน ประวัติพลังงาน และสถานะผิดปกติผ่าน Web UI

### Stage 8 — ปรับความถูกต้องของ Battery และ Energy Period (30 ตุลาคม – 3 พฤศจิกายน 2026)
- แก้การตั้งค่าแรงดัน Battery ให้รองรับ 24V/48V
- แก้ monthly reset สำหรับช่วงวันที่คร่อมเดือน
- ให้ความสำคัญกับ boundary condition เพราะมีผลกับ Energy History และสรุปรายเดือน

### Stage 9 — เปลี่ยนชื่อและจัดระเบียบเป็น HEM (4 พฤศจิกายน 2026)
- เปลี่ยนชื่อจาก Hybrid Inverter เป็น **Hybrid Energy Manager (HEM)**
- เปลี่ยนชื่อโฟลเดอร์เป็น `D:\@Project\Hybrid-Energy-Manager`
- เปลี่ยน GitHub repository เป็น `Artputeye/Hybrid-Energy-Manager`
- ปรับ Git remote, README และชื่อ Dashboard
- ตรวจสอบ Local `main` กับ GitHub `main` ให้ตรงกัน
- จัดทำ `plan.md` และ `struc.md` เพื่อเก็บประวัติและโครงสร้าง

## 4. ความสามารถที่สะสมมาถึงปัจจุบัน

รายการนี้เป็นภาพรวมจาก README, source structure และบริบทการพัฒนา ไม่ได้หมายความว่าทุกฟังก์ชันถูกเพิ่มใน commit เดียวกัน

### 4.1 Inverter Communication
- เชื่อมต่อ Anern Hybrid Inverter 4.2kW ผ่าน ESP32 Serial2/UART
- Serial setting: 2400 baud, 8N1
- กลุ่มคำสั่งอ่านข้อมูล: `QPIGS`, `QPIRI`, `QPIWS`, `QFLAG`, `QDI`, `QMOD`
- แยก Inverter communication ออกจาก control logic
- อ่านค่ากำลังไฟ แรงดัน กระแส ความถี่ PV, Battery และ Fault

### 4.2 Inverter Control
- แยกส่วนควบคุม Inverter จาก driver
- รองรับการเขียน register/คำสั่งควบคุมตาม protocol
- มีการตั้งค่า Grid Tie Operation และพารามิเตอร์ Inverter
- มีแนวทาง Grid Auto อาศัยพลังงานและเงื่อนไขที่กำหนด

### 4.3 Network
- Wi-Fi Station และ AP configuration mode
- Wi-Fi reconnect
- ตั้งค่าเครือข่ายและ MQTT broker
- HTTP server และ mDNS
- แยก Network Manager ออกจาก Application logic

### 4.4 Web Dashboard
- เสิร์ฟ Web UI จาก LittleFS
- หน้า Dashboard, Battery, Settings, Network, State, Info, OTA และ File List
- WebSocket endpoint `/ws` สำหรับข้อมูล real-time
- แสดง Inverter, Power, Energy และ Fault
- มี Expense Settings, Energy Cost และ Dashboard Summary

### 4.5 MQTT / Home Assistant
- ใช้ PubSubClient เป็น MQTT client
- มี Home Assistant Discovery/Integration
- ส่งสถานะ Inverter, Grid, PV, Battery, Power และ Energy
- แยก implementation ไว้ใน `ha_integration`

### 4.6 Energy / Cost / History
- Energy Tracker และ Energy History
- การสรุปข้อมูลรายชั่วโมง รายวัน และรายเดือนตามที่ออกแบบไว้
- Energy Cost และ progressive/tier electricity rate
- Dashboard Summary
- แก้ monthly range ที่คร่อมเดือนแล้ว

### 4.7 Battery / Configuration
- Battery settings แยกจาก Inverter settings
- รองรับการตั้งค่าระบบ Battery 24V/48V
- เก็บค่าตั้งค่าผ่าน JSON และ LittleFS ตาม flow ของโปรเจกต์

### 4.8 OTA และ Runtime
- OTA firmware และ LittleFS
- Partition table สำหรับ OTA
- FreeRTOS tasks แยกงานหลัก, network/MQTT/WebSocket, LED และ Serial console
- NTP และ timezone GMT+7 สำหรับเวลาและ Energy period

## 5. สถานะปัจจุบัน

ผู้ใช้ยืนยันว่า **โปรแกรมปัจจุบันทำงานได้ดีอยู่แล้ว** ดังนั้นให้ถือสถานะปัจจุบันเป็น Working Baseline ไม่ควรตีความสิ่งที่ไม่ได้กล่าวถึงใน Git log ว่าเป็น Bug หรือฟีเจอร์ที่ยังไม่เสร็จ

| หัวข้อ | สถานะ |
|---|---|
| Firmware / PlatformIO | มี baseline build ผ่าน |
| Inverter communication | เป็นฟังก์ชันหลักที่พัฒนามาแล้ว |
| Wi-Fi / AP / Network | มีการพัฒนาและแก้ปัญหา reconnect/AP |
| Web Dashboard / Settings | พัฒนาและปรับ UI หลายรอบ |
| MQTT / Home Assistant | เป็นส่วนหนึ่งของระบบ |
| Energy Tracking / History | เพิ่มและปรับปรุงแล้ว |
| Energy Cost / Progressive Tariff | เพิ่มแล้ว |
| Fault Monitoring via WebSocket | เพิ่มแล้ว |
| Battery 24V/48V | มี commit แก้ไขแล้ว |
| Monthly cross-month handling | มี commit แก้ไขแล้ว |
| Project naming / GitHub | เปลี่ยนเป็น HEM แล้ว |

## 6. งานที่ยังไม่เสร็จ / งานที่ตั้งใจทำต่อ

บันทึกเฉพาะงานที่ผู้ใช้ยืนยันว่าค้างอยู่หรือมีการตกลงว่าจะทำต่อเท่านั้น

| วันที่บันทึก | งาน | สถานะ | หมายเหตุ |
|---|---|---|---|
| — | ยังไม่มีรายการที่ยืนยันในเอกสารฉบับนี้ | — | เพิ่มเมื่อมีการระบุงานค้างจริง |

รูปแบบบันทึกงาน:
- **วันที่พบ:** YYYY-MM-DD
- **ส่วนของระบบ:** module/page/function
- **อาการหรือเป้าหมาย:** สิ่งที่ยังไม่เสร็จ
- **สถานะ:** Pending / In Progress / Done / Deferred
- **ผลการแก้ไข:** commit หรือไฟล์ที่เปลี่ยนเมื่อเสร็จ

## 7. ลำดับการพัฒนาที่ถอดบทเรียนไปใช้กับโปรเจกต์ใหม่

1. **Hardware + Communication:** ระบุบอร์ด, pin, protocol, baud rate, frame และวิธีทดสอบ
2. **Firmware Core:** แยก driver จาก control logic และกำหนด timeout/error handling
3. **Network:** เพิ่ม AP/STA, reconnect และ network configuration หลัง Core ทำงานได้
4. **Storage/Configuration:** กำหนด schema, defaults, persistence และ recovery
5. **Web API/Dashboard:** เริ่มจากข้อมูลจำเป็น แล้วเพิ่ม real-time updates
6. **MQTT/Home Assistant:** กำหนด topic, entity, unit และ discovery
7. **Energy/Analytics:** ระบุหน่วยและสูตร พร้อมทดสอบ boundary ของวัน/เดือน
8. **Automation/Control:** แยกเงื่อนไขตัดสินใจจาก transport และกำหนด safety limits
9. **OTA/Maintenance:** วาง partition, update flow และ recovery
10. **Git/Documentation:** บันทึกทุก milestone ด้วย commit message ที่สื่อว่าเพิ่มอะไรหรือแก้ปัญหาใด

ลำดับนี้เป็นบทเรียนจาก HEM ไม่ใช่ข้อกำหนดว่าทุกโปรเจกต์ต้องมีทุก subsystem

## 8. คำสั่งอ้างอิง

```powershell
# Build firmware
pio run

# Upload firmware
pio run --target upload

# Upload LittleFS
pio run --target uploadfs

# Serial monitor
pio device monitor -b 115200

# Git history (เรียงจากเริ่มต้น)
git log --reverse --date=short --pretty=format:"%h %ad %s"

# Git status / diff
git status --short --branch
git diff
```

## 9. กติกาการดูแลเอกสาร

- เมื่อมีฟีเจอร์ใหม่ ให้เพิ่ม milestone พร้อมวันที่และ commit ใน Section 2 และ 3
- เมื่อแก้ Bug ให้บันทึกอาการ สาเหตุ และแนวทางแก้ ไม่บันทึกเพียงคำว่า Fix
- เมื่องานที่ยืนยันว่าค้างเสร็จ ให้เปลี่ยนสถานะและเก็บประวัติไว้
- เมื่อเปลี่ยน architecture, library หรือ folder structure ให้ปรับ `struc.md`
- เก็บประวัติเดิมไว้ ไม่เขียนทับ timeline ด้วยสถานะล่าสุด
- ห้ามบันทึก Wi-Fi password, MQTT password, token หรือ secret ลงเอกสาร
- ห้าม commit/push หรือเปลี่ยน Git history โดยไม่ได้รับอนุญาต

---

**สรุป:** HEM เริ่มจาก ESP32 Inverter Controller ในเดือนกรกฎาคม 2026 และค่อย ๆ เติบโตเป็นระบบจัดการพลังงานที่รวม Inverter communication, Network, Web Dashboard, MQTT/Home Assistant, Energy Tracking, Energy Cost, History, Fault Monitoring และ Battery configuration จนเปลี่ยนชื่อเป็น Hybrid Energy Manager ในวันที่ 4 พฤศจิกายน 2026
