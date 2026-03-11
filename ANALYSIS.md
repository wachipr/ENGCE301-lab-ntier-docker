# 📊 การวิเคราะห์เปรียบเทียบ: VM vs Docker Deployment
## ENGCE301 - Week 6 N-Tier Architecture

**ชื่อ-นามสกุล:** เมธิวัฒน์ ชมชื่น  
**รหัสนักศึกษา:** 67543206069-6
**วันที่:** 26 ก.พ. 2569

---

## 1. ตารางเปรียบเทียบ Setup Process

| ขั้นตอน | Version 1 (VM) | Version 2 (Docker) |
|---------|----------------|-------------------|
| ติดตั้ง PostgreSQL | `sudo apt install postgresql` (5-10 นาที) | ใช้ Base Image `postgres:16-alpine` ดาวน์โหลดอัตโนมัติ |
| ติดตั้ง Node.js | `nvm install 20` + PM2 (5 นาที) | สร้างผ่าน Dockerfile `FROM node:20-alpine` |
| ติดตั้ง Nginx | `sudo apt install nginx` (2-3 นาที) | ใช้ Base Image `nginx:alpine` |
| Configure Database | แก้ไข `pg_hba.conf`, `postgresql.conf` และสร้าง User (10-15 นาที) | ตั้งค่า Environment Variables ใน `docker-compose.yml` |
| Configure SSL | สร้าง cert และติดตั้งใน `/etc/nginx/sites-available` (10-15 นาที) | สร้าง cert และ Mount เข้า Nginx Container เป็น Volume |
| Start Services | สั่ง `systemctl restart` และ `pm2 start` (2-3 นาที) | รันคำสั่ง `docker compose up -d` ครั้งเดียว |
| **เวลาทั้งหมด** | 34-51 นาที | 1-5 นาที (ขึ้นอยู่กับอินเทอร์เน็ตในการโหลด Image) |

---

## 2. ตารางเปรียบเทียบ Resource Usage

| Resource | Version 1 (VM) | Version 2 (Docker) |
|----------|----------------|-------------------|
| Memory Usage | ~500 MB - 1 GB (รวมระบบปฏิบัติการ) | ~100 MB - 150 MB (เฉพาะ Containers) |
| Disk Usage | ~5 GB - 10 GB (พื้นที่ของ Ubuntu และ Dependencies) | ~400 MB - 600 MB (ขนาดรวม Images) |
| CPU Usage | 2% - 5% (Background OS process) | < 1% (ไม่มี Overhead ของ OS) |
| Startup Time | 30 - 45 วินาที (บูตระบบและ Service) | 2 - 5 วินาที (เริ่มรัน Container ทันที) |

---

## 3. ข้อดีของ Docker Deployment (เขียน 5 ข้อ)

1. **Deploy ง่ายและเร็ว:** สามารถขึ้นระบบได้ทั้งหมดภายใน 1 คำสั่ง `docker compose up -d` ทำให้ประหยัดเวลาอย่างมากเทียบกับการใช้ VM
2. **สภาพแวดล้อมสม่ำเสมอทุกเครื่อง (Consistency):** ลดปัญหาการที่โปรแกรมรันบนเครื่องผู้พัฒนาได้แต่ทำงานบนโปรดักชันไม่ได้ ("Works on my machine!")
3. **เคลื่อนย้ายสะดวก (Portability):** การส่งมอบและย้ายระบบให้เครื่องอื่นทำได้ง่ายด้วยไฟล์ `docker-compose.yml` ร่วมกับ Docker Hub
4. **ทำความสะอาด (Cleanup) ง่าย:** หากไม่ต้องการใช้งานแล้วสามารถจัดการลบ Container, Network และ Volume ทั้งหมดทิ้งได้ด้วย `docker compose down -v` ไม่มี Environment ฝังลึกในเครื่อง Host
5. **ดึงประสิทธิภาพเครื่องสูงสุด (Lightweight):** ลด Overhead ที่ไม่ต้องการ เพราะข้ามการรันระบบปฏิบัติการซ้อน ใช้อุปกรณ์ Hardware ทรัพยากรร่วมกับ Host OS ได้โดยตรง

---

## 4. ข้อเสียของ Docker Deployment (เขียน 3 ข้อ)

1. **การเรียนรู้เทคโนโลยีใหม่ (Learning Curve):** ผู้ดูแลระบบต้องศึกษาถึงคำสั่งเฉพาะทางของ Docker, การสร้าง Dockerfile รวมถึงสถาปัตยกรรมเครือข่ายของ Container ซึ่งมีความซับซ้อนกว่าในระยะเริ่มต้น
2. **การจัดการข้อมูลถาวร (Data Persistence):** แม้ Container สามารถเปิดและปิดได้ง่าย แต่ถ้าไม่ได้ Mount Volume เอาไว้ ข้อมูลภายในเช่น ฐานข้อมูลหรือรูปภาพต่างๆ จะสูญหายไปหลังการลบทันที
3. **การ Debug และจัดการ Network:** มักมีความยุ่งยากในการเชื่อมต่อหรือเปิด Port เพราะแต่ละ Container อยู่ในเครือข่ายภายใน (Bridge Network) ของตนเอง การตรวจสอบทราฟฟิกจะท้าทายกว่า VM ปกติ

---

## 5. เมื่อไหร่ควรใช้ VM vs Docker?

### ควรใช้ VM เมื่อ:
- แอปพลิเคชันต้องการระบบปฏิบัติการ หรือ Kernel เจาะจงเป็นการส่วนตัว (เช่น ใช้งาน Windows Application หรือบริการ Linux บางประเภทเฉพาะ)
- ต้องการความปลอดภัยและความเป็นส่วนตัวแบบการกั้น (Isolation) ในแง่ Infrastructure ระดับที่สูงกว่า Docker
- เมื่อต้องใช้งานกับระบบแบบเก่า (Legacy System) ซึ่งไม่สามารถปรับโครงสร้างเป็นแบบ Microservices ได้

### ควรใช้ Docker เมื่อ:
- ใช้โครงสร้างและสถาปัตยกรรมแบบ N-Tier Architecture หรือ Microservices ที่ต้องการแบ่งสัดส่วนการให้บริการให้ชัดเจน
- โปรเจกต์ที่ต้องการระบบ CI/CD และ Pipeline อัตโนมัติที่มีประสิทธิภาพและเคลื่อนย้ายคล่องตัวสูงสุด
- ต้องการลดต้นทุนค่าใช้จ่ายด้านเซิร์ฟเวอร์ด้วยการใช้ทรัพยากรการคำนวณและหน่วยความจำบน Cloud เท่าที่จำเป็น

---

## 6. สิ่งที่ได้เรียนรู้จาก Lab นี้

จากการทดลองปฏิบัติการในครั้งนี้ ได้เรียนรู้ความแตกต่างระหว่างสถาปัตยกรรมแบบเดิมบน Virtual Machine และสถาปัตยกรรมสมัยใหม่บน Container ของ Docker เครื่องมือนี้ช่วยให้สามารถแยกเลเยอร์ Database, API และ Reverse Proxy ออกจากกันได้อย่างเป็นสัดส่วน พร้อมทั้งอำนวยความสะดวกในการ Deploy ด้วย Command เพียงชุดเดียว นอกจากนี้ยังได้ฝึกเขียนภาพรวมเพื่อจำลองสถานการณ์การขึ้นระบบจริงบน Docker Compose อีกด้วย

---

## 7. คำสั่ง Docker ที่ใช้บ่อย (Quick Reference)

```bash
# รายละเอียดคำสั่งที่ใช้งานบ่อย ๆ ใน Docker
docker compose up -d                 # รันทุก Container ใน Background Mode
docker compose down -v               # หยุดการทำงานของ Container, ลบ Network และ Volumes
docker ps                            # ดูสถานะและรายการ Container ทั้งหมดที่กำลังทำงานอยู่
docker --version                     # เช็คเวอร์ชันปัจจุบันของ Docker Engine
docker compose logs -f api           # ดู Log แบบ Stream ของ Service API (เพื่อดีบัก)
docker system df                     # ตรวจสอบการใช้พื้นที่หน่วยความจำและดิสก์ของ Docker