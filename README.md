# engce301-termproject-week6-ntier-docker

**ชื่อ-นามสกุล:** เมธิวัฒน์ ชมชื่น  
**รหัสนักศึกษา:** 67543206069-6

## บันทึกผลการทดลอง

ใน Repository นี้ ประกอบด้วยการทดลอง 2 สัปดาห์รวมกัน ได้แก่

| # | หัวข้อ | บันทึกผลการทดลอง |
|------|-------|------|
| Week 6 | N-Tier Architecture | [ANALYSIS.md](ANALYSIS.md) |
| Week 7 | Deploy TaskBoard to Cloud | [CLOUD_DEPLOYMENT.md](CLOUD_DEPLOYMENT.md) |

---

![alt text](./.github/screenshots/2.png)

รูปภาพสำหรับ Week 7 สามารถดูได้ที่ [.github/screenshots](./.github/screenshots)

## สรุปความรู้ที่ได้
- **การทำ N-Tier Architecture บน Docker (Week 6):** ได้ทำความเข้าใจถึงประโยชน์ของการใช้ Container ในการแยกสัดส่วนการให้บริการ (เช่น API, Database, และ Reverse Proxy) ออกจากกัน ซึ่งแก้ไขปัญหา Resource หมกมุ่น ลดเวลา Setup และขจัดปัญหา "Works on my machine!" ทำให้เกิด Environment ที่สม่ำเสมอและคล่องตัวเมื่อเทียบกับการ Deploy บน Virtual Machine (VM)
- **การนำโครงสร้างไปรันบนแพลตฟอร์ม PaaS Cloud (Week 7):** ได้ประยุกต์ใช้ความรู้จาก Week 6 ในการแบ่ง Service ไป Deploy ขึ้นระบบ Cloud (Railway) ซึ่งช่วยอำนวยความสะดวกในฐานะนักพัฒนาด้วยสถาปัตยกรรมแบบ PaaS อย่างการไม่ต้องจัดการโครงสร้างพื้นฐาน (Server/OS/SSL) ด้วยตนเอง
- **12-Factor App:** ได้เรียนรู้แนวทางปฏิบัติที่ดีให้เหมาะสมกับ Cloud-Native Application เช่นการนำ Configuration และ URL ออกมาไว้ใน Environment Variables, รวมถึงหลักการ Codebase และ Logging ที่มีความสำคัญต่อความปลอดภัยและการทำ CI/CD ในอนาคต
- **ข้อพิจารณาในการเลือกใช้เทคโนโลยี:** เข้าใจในข้อจำกัดและข้อได้เปรียบว่าควรเลือกใช้ Docker Containers ในกระบวนการ Development/Testing เพื่อควบคุม Software ได้อย่างเบ็ดเสร็จ และนำระบบโปรเจกต์เหล่านี้ขึ้น Cloud Platform (PaaS) เมื่อเข้าสู่โหมด Production เพื่อตอบสนองผู้ใช้งานด้วย Uptime และการ Scaling ที่มีประสิทธิภาพสูงสุด
