# 🔗 Cisco EtherChannel (PAgP - Desirable Mode)

## 🧠 EtherChannel คืออะไร?
**EtherChannel** คือเทคนิคการรวมพอร์ตหลายเส้นให้กลายเป็นเส้นเดียวแบบ Logical  
- เพิ่มความเร็ว (Bandwidth)
- ป้องกัน Loop โดยไม่ต้องใช้ STP ปิดพอร์ต
- ใช้งานร่วมกับ PAgP หรือ LACP

---

## ✅ จุดประสงค์ของการตั้งค่านี้
- รวมพอร์ต 2 พอร์ตเป็น EtherChannel
- ใช้ **PAgP แบบ Desirable Mode**
- ตั้งค่าให้เป็น **Trunk Port** เพื่อส่ง VLAN ได้หลาย VLAN

---
 ## เงื่อนไขสำคัญ
- พอร์ตที่รวมเป็น EtherChannel ต้อง:
  - ความเร็วเท่ากัน
  - Duplex ตรงกัน
- อีกฝั่ง (เช่น Switch A) ต้องตั้งค่าให้เข้ากัน เช่น mode auto หรือ mode desirable
- VLAN ที่ต้องการส่งต้องถูกกำหนดไว้ทั้งสองฝั่ง

---
## การใช้งานจริง
คุณมี 2 สวิตช์ คือ Switch A และ Switch B ต้องการเชื่อมต่อกันเพื่อ:
- เพิ่มความเร็วระหว่างสวิตช์
- ป้องกัน Loop
- ส่งหลาย VLAN ระหว่างกัน (Trunk)

---
## 🖧 การเชื่อมต่อ:
- มีการใช้สาย Ethernet 2 เส้น เชื่อมระหว่าง Switch A กับ Switch B บนพอร์ต:
- Switch A: Ethernet 0/0, 0/1
- Switch B: Ethernet 0/0, 0/1

|Switch A |                     | Switch B|
| ------------------------------------ |
|e0/0 | ---------------------   | e0/0 |
|e0/1 | ---------------------   | e0/1|


---
## 🎯 ผลลัพธ์
- สาย 2 เส้นรวมเป็นเส้นเดียวแบบ Logical
- แบนด์วิธเพิ่มขึ้น (เช่น 2 Gbps แทน 1 Gbps ถ้าใช้สาย Gigabit)
- Trunk ส่งข้อมูล VLAN ต่างๆ ได้ผ่าน Port-channel1
- ลดการใช้ STP ปิดพอร์ต เพราะ EtherChannel ถูกมองเป็นลิงก์เดียว

---
## สรุป
การใช้งาน EtherChannel ในโลกจริงมักใช้กับการเชื่อมต่อ:
- ระหว่าง Core Switch <-> Access Switch
- ระหว่าง Switch <-> Router (ที่รองรับ EtherChannel)
- เพื่อ เพิ่มประสิทธิภาพ, ลดโหลด, ป้องกันปัญหา STP ปิดพอร์ต

---
## อธิบายคำสั่งแบบเข้าใจง่าย
| คำสั่ง                                 | ความหมาย                                                           |
| -------------------------------------- | ------------------------------------------------------------------ |
| `interface range e0/0 - 1`             | เลือกพอร์ต Ethernet 0/0 และ 0/1                                    |
| `channel-group 1 mode desirable`       | รวมพอร์ตเข้ากลุ่ม EtherChannel หมายเลข 1 ด้วยโหมด Desirable (PAgP) |
| `no shutdown`                          | เปิดใช้งานพอร์ต (หากยังถูกปิดอยู่)                                 |
| `interface Port-channel1`              | เข้าไปตั้งค่าพอร์ตแบบรวม (Logical) ที่ได้จาก EtherChannel          |
| `description Link to Switch A`         | ใส่คำอธิบายให้เข้าใจง่าย (ไม่ส่งผลต่อการทำงาน)                     |
| `switchport trunk encapsulation dot1q` | ใช้ trunk แบบมาตรฐาน IEEE 802.1Q                                   |
| `switchport mode trunk`                | กำหนดให้เป็น trunk เพื่อส่ง VLAN ได้หลาย VLAN                      |
