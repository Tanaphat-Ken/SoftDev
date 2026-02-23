# Non-Functional Testing (jMeter) and Defect Tracking

## Non-Functional Testing (jMeter)
### Non-Functional (ให้ระบบเสถียรและมีประสิทธิภาพ)
(ถ้า Input Output ชัดเจนเป็น Functional)
- Performance Testing
- Scalability Testing (Vertical Scaling - HW, Horizontal Scaling - VM)
- Availability Testing (พวก One nine, Two nines, Three nines, Four nines, Five nines)
- Security Testing
- Latency Testing
- ...

### What is jMeter?
- Open source software by Apache
- Load and performance testing tool (ยิง Traffic ไปยังระบบ)
- วัด App Performance
- Scriptable
- Realtime Results
- Analyzes and reports data

### Why do we need jMeter?
- Improve Non-Functional Aspects of Software
- load and stress testing

### Benefits
- Free open source tool
- Supports หลาย protocols (HTTP, SMTP, etc.)
- Customizable
- Playback feature

### Thread Group & Sampler
- 1 Thread = 1 User
- Thread Group simulates user to request to server
- ขึ้นตาม cpu core ที่มีอยู่ในเครื่อง (ถ้าเครื่องมี 4 core ก็จะมี 4 thread)
- Sampler คือการกำหนดว่าแต่ละ thread จะทำอะไร (เช่น HTTP Request, FTP Request, etc.)
- jMeter Create users with sampler to send request to server
- สามารถกำหนด thread, ramp-up time, loop count ได้
- Thread Count = จำนวนผู้ใช้งานที่ต้องการจำลอง (Parallel Users)
- Loop Count = Sequential Users (จำนวนครั้งที่แต่ละ thread จะทำงาน)
- Ramp-Up Time = เวลาที่ต้องการให้ thread ทั้งหมดเริ่มทำงาน (เช่น 10 วินาที = 10 thread จะเริ่มทำงานภายใน 10 วินาที)
- Throughput = Requests per minute (ยิ่งเยอะยิ่งดี)
- Deviation = ความแปรปรวนของ response time (ยิ่งน้อยยิ่งดี)
- Average = ค่าเฉลี่ยของ response time (ยิ่งน้อยยิ่งดี) (ใกล้กับ Median ยิ่งดี)
- Median = ค่ากลางของ response time (ยิ่งน้อยยิ่งดี) (ใกล้กับ Average ยิ่งดี)

### Performance Testing
- Use jMeter to simulate load on server
- If jMeter เกิดสิ่งผิดปกติ ไปใช้ Server Monitoring Tools เช่น Grafana, Prometheus เพื่อดูว่าเกิดอะไรขึ้นกับ server
- Response Time/User ขึ้นเป็น exponential แสดงว่า server มีปัญหาในการจัดการกับ load
- ถ้า RT/User ขึ้นเป็น linear แสดงว่า server สามารถจัดการกับ load ได้ดี หรือขึ้นแล้วเป็น plateau แสดงว่า server จัดการกับ load ได้ดี ณ ตอนนั้น
- RT/User ขึ้นแล้วล่วงเลย + plateau ด้านล่าง แสดงว่า Server น่าจะ down (return error code ต่างๆ)
- CPU/Time ขึ้น 100% แสดงว่า Code Big O สูงเกิน
- RAM/Time ขึ้นสูง แสดงว่า Code มี Memory Leak, พวกการ load ต่างๆเข้า RAM
- Disk I/O/Time ขึ้นสูง แสดงว่า Code มีการเขียนอ่านไฟล์เยอะเกินไป
- Network I/O/Time ขึ้นสูง แสดงว่า Code มีการส่งข้อมูลเยอะเกินไป ไปใช้พวก server นอกเพื่อช่วยลด load ได้

## Defect Tracking
### Software Testing
- ทดสอบ Software เพื่อหาข้อผิดพลาด (Defect) และแก้ไขให้ถูกต้อง
- ตรวจสอบ performance
- quality
- ส่วนใหญ่ Testers ทำ
- Dev และ User ก็มีทำ
- มีแบบแผนการทดสอบ

### Defect Types
- Requirement -> เกิดจากการแก้ Requirement
- Coding -> เกิดจากการที่ Dev ไม่ตรวจเช็ค
- Graphic Design -> Design ไม่รองรับ browser หรือ Device ต่างๆ
- Data Test -> Test data อาจไม่รองรับ
- Other -> ข้อจำกัดของระบบ, ENV

### Defect Severity
- Critical -> ทำให้ระบบล่ม, ข้อมูลเสียหาย, ไม่สามารถใช้งานได้เลย, Function พัง (เสียหายหนัก พวกเงินๆทองๆ) (Show Stopper)
- High -> ใส่ข้อมูลถูก แต่ระบบแสดงผลผิด (มีผลกระทบต่อ UX หรือ Functionality ที่สำคัญ) (ถึงขั้นธุรกิจ เงินๆทองๆ)
- Medium -> เมื่อใส่ข้อมูลผิด ระบบจะแสดงผลผิดพลาด (พวก Display ไม่ค่อยดี)
- Low -> ข้อผิดพลาดเล็กน้อยที่ไม่ส่งผลกระทบต่อการใช้งาน (พวก Typo, UI ไม่ค่อยดี, พวก Nice to have)

### Test Document
- Planning Test -> Perform Tests -> Document Results
- มีการเก็บข้อมูล, วางแผน, รายงาน

### Defect Log
- Description -> Test item ID, ENV description
- Activity and event entries
  - Date and time
  - Author 
  - Test Procedure identifier
  - Staff present
  - Pass/Fail
  - Error message
  - ENV info
  - Anomalous events

### Defect Tracking
- ลบไม่ได้ แต่แก้ไขได้ (Status)

### Defect Status Flow
- Defect ถึงลูกค้า -> Tester โดนเล็ง
```
NEW -> ACCEPT -> RESOLVED -> VERIFIED (Happy case)
REJECTED -> REOPEN (Dev มีปัญหา)
```