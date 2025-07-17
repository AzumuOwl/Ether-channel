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

 ## เงื่อนไขสำคัญ
- พอร์ตที่รวมเป็น EtherChannel ต้อง:
  - ความเร็วเท่ากัน
  - Duplex ตรงกัน
- อีกฝั่ง (เช่น Switch A) ต้องตั้งค่าให้เข้ากัน เช่น mode auto หรือ mode desirable
- VLAN ที่ต้องการส่งต้องถูกกำหนดไว้ทั้งสองฝั่ง
