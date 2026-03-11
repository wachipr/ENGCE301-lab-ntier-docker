# Cloud Deployment Analysis
## ENGCE301 - Week 7 Lab

**ชื่อ-นามสกุล:** เมธิวัฒน์ ชมชื่น
**รหัสนักศึกษา:** 67543206069-6

### 1.1 URLs ของระบบที่ Deploy

| Service | URL |
|---------|-----|
| Frontend | https://frontend-production-89b2.up.railway.app |
| Backend API | https://api-service-production-9e8a.up.railway.app |
| Database | (Internal - ไม่มี public URL) |

### 1.2 Screenshot หลักฐาน (5 รูป)

1. [x] Railway Dashboard แสดง 3 Services
![alt text](./.github/screenshots/1.png)
2. [x] Frontend ทำงานบน Browser
![alt text](./.github/screenshots/2.png)
3. [x] API Health check response
![alt text](./.github/screenshots/3.png)
4. [x] Logs แสดง requests
![alt text](./.github/screenshots/4.png)
5. [x] Metrics แสดง CPU/Memory
![alt text](./.github/screenshots/5.png)

### 2.1 ความแตกต่างที่สังเกตเห็น (10 คะแนน)

| ด้าน | Docker (Week 6) | Railway (Week 7) |
|------|-----------------|------------------|
| เวลา Deploy | 1-5 นาที (ขึ้นอยู่กับความเร็ว Internet และสเปคเครื่องตอน Build) | รวดเร็วผ่านระบบ Auto Deploy เพียงการ Push Code ขึ้น GitHub |
| การตั้งค่า Network | กำหนดเองผ่านไฟล์ docker-compose.yml และตั้งค่า Reverse Proxy เอง | ระบบ Railway จัดการ Internal Network แยะแยกระหว่าง Public URLs อัตโนมัติ |
| การจัดการ ENV | โหลดจากไฟล์ `.env` ที่อยู่ในโฟลเดอร์ Repository หลัก | ทำได้ง่ายผ่านแท็บ Variables และใช้ References (เช่น `${{Postgres.DATABASE_URL}}`) |
| การดู Logs | ใช้คำสั่ง `docker compose logs` เพื่อตรวจสอบ | ดูแบบ Real-time บน Dashboard ได้เป็นราย Service |
| การ Scale | แก้ไข Configuration เป็น manual scale เบื้องต้น | เพิ่ม-ลด Resource อัตโนมัติ (Auto-scaling) ผ่าน UI Slider ง่ายๆ |

### 2.2 ข้อดี/ข้อเสีย ของแต่ละแบบ (5 คะแนน)

**Docker Local:**
- ข้อดี: ฟรี ไม่เสียค่าใช้จ่าย สามารถควบคุมทรัพยากรทุกอย่างด้วยตนเอง เหมาะสำหรับเรียนรู้ ทดสอบและตรวจสอบการทำงานใน Development phase ได้ 100%
- ข้อเสีย: ดูแลรักษาและจัดการ Backup ด้วยตนเอง อีกทั้งระบบต้องทำงานบนเครื่อง Local ทำให้เข้าถึงได้แค่จากเครื่องตนเอง (นอกจากต้องเปิด Port ให้ออกภายนอกซึ่งมีความเสี่ยง)

**Railway Cloud:**
- ข้อดี: นำระบบขึ้น Production ได้อย่างรวดเร็ว ได้รับประโยชน์จากฟีเจอร์ PaaS (ไม่ต้องคอยดูแล Server, SSL อัตโนมัติ, Uptime สูง, Deployments History เพื่อการย้อนกลับ)
- ข้อเสีย: มีค่าใช้จ่ายต่อเดือนหากใช้งานเกิน Free Credits และไม่สามารถเข้าถึง Host OS ระดับล่างเพื่อปรับแต่งได้ (ถูกซ่อนความซับซ้อนไปแล้ว)

### 3.1 Railway เป็น Service Model แบบไหน?

[ ] IaaS   [x] PaaS   [ ] SaaS

เพราะ: Railway ทำหน้าที่จัดเตรียมสภาพแวดล้อมสำหรับรันแอปพลิเคชัน (Platform as a Service) อย่างเช่น Node.js รวมถึง Database ให้พร้อมใช้งานทันที โดยที่นักพัฒนาเพียงแค่นำ Source Code และข้อมูลไปวางไว้โดยไม่ต้องยุ่งยากกับการติดตั้งระบบปฏิบัติการ (OS) ระบบความปลอดภัย เครือข่าย ไปจนถึงการขอ Certificate (SSL) แต่อย่างใด

### 3.2 ถ้าใช้ IaaS (เช่น AWS EC2) ต้องทำอะไรเพิ่มอีก? (ยกตัวอย่าง 4 ข้อ)

1. จัดหา ติดตั้ง และอัปเดตระบบปฏิบัติการ (OS) รวมถึงเซิร์ฟเวอร์ด้วยตนเอง
2. Config และตั้งค่าระบบ Network รวมถึงจัดการ Firewall อย่างเต็มรูปแบบ
3. ทำการหาและติดตั้ง SSL Certificates สำหรับใช้งาน HTTPS ด้วยตนเอง
4. วางระบบและจัดการฐานข้อมูล (Database) รวมถึงแผนการ Backup และ Scaling อย่างเป็นระบบด้วนตนเอง

### 4.1 Factors ที่เห็นจาก Lab (10 คะแนน)

เลือก 5 Factors และอธิบายว่าเห็นจากไหนใน Railway:

| Factor | เห็นจากไหน? | ทำไมสำคัญ? |
|--------|------------|-----------|
| Factor 3: Config | Variables tab | เพื่อแยกโค้ดหลักออกจากค่าความปลอดภัย เช่น รหัสการเข้ารหัสหรือที่อยู่เชื่อมต่อ Database ป้องกัน Credentials รั่วไหล |
| Factor 1: Codebase | Deploy จาก GitHub Repo | ทำให้มั่นใจว่า Source Code หลักอยู่ที่เดียวกัน เสมอภาค และเป็น Tracking ใช้อ้างอิงสำหรับการทำ CI/CD ต่อ |
| Factor 4: Backing Services | Add PostgreSQL แล้วสร้างเป็น Attached Resource ทันที | ทุก Service ต้องสื่อสารผ่าน Resource URL เผื่อในวันข้างหน้าขยายและใช้ของแยกจาก Provider ภายนอกก็ทำได้ง่าย |
| Factor 11: Logs | ล็อกการทำงานทุก Service รวมศูนย์ใน Logs tab บน Service Dashboard | แสดงและแคปเจอร์ stdout / stderr ต่อไป เพื่อการค้นหาความผิดปกติของข้อมูลระบบ (Observability) ที่ใช้งานได้จริง |
| Factor 5: Build, Release, Run | แถบ Deployments History | ทำให้การทำ Rollback ง่าย ติดตามสถานะของแต่ละบิลด์ ทุกการเปลี่ยนแปลงจะมีรุ่นที่แน่นอน |

### 4.2 ถ้าไม่ทำตาม 12-Factor จะมีปัญหาอะไร? (5 คะแนน)

ยกตัวอย่าง 2 ปัญหา:

**ปัญหา 1:** ถ้าไม่ทำตาม Factor 3 (Config)
- สิ่งที่จะเกิด: หากใช้ Hardcode ใส่รหัสสำหรับฐานข้อมูลลงไปใน Source Code รหัสอาจหลุดไปถึงผู้พัฒนาคนอื่นๆ หรือกลายเป็นเรื่องยากเมื่อระบบเปลี่ยน Environment เพราะต้องตามแก้ไฟล์ Code อย่างเดียว ทำให้ระบบเกิดจุดอ่อนด้าน Cybersecurity ขึ้นได้

**ปัญหา 2:** ถ้าไม่ทำตาม Factor 6 (Stateless Processes)
- สิ่งที่จะเกิด: ถ้า Process เก็บบันทึกข้อมูลจำเพาะของผู้ใช้งานหรือ Session ไว้บน Memory เซิร์ฟเวอร์ตัวเอง เมื่อเซิร์ฟเวอร์ระบบเกิดพังไป ข้อมูลทุกอย่างก็จะสูญหายไป นอกจากนี้ยังทำให้เป็นอุปสรรคของการเพิ่ม Load Balancer Scale Application ในอนาคตไม่ได้ด้วย

### 5.1 สิ่งที่เรียนรู้จาก Lab นี้

1. เข้าใจบทบาทและข้อแตกต่างของ IaaS, PaaS และ SaaS ควบคู่ไปกับการฝึกใช้งาน Cloud แนว PaaS บนระบบเซิร์ฟเวอร์จริงนอกเหนือจากบนเครื่องตัวเอง
2. เรียนรู้วิธีการทำ N-Tier Architecture บน Cloud ผ่านกระบวนการแก้ไขค่า Environment และเพิ่ม CORS Origin เบื้องต้น
3. ได้ตระหนักถึงหลักการออกแบบระบบในอุดมคติตามแนวคิด 12-Factor App ผ่านประสบการณ์ตรงของการจัดการ Config, Codebase, Backend, และ Logs

### 5.2 ความท้าทาย/ปัญหาที่พบ และวิธีแก้ไข

ปัญหา: การเชื่อมต่อฐานข้อมูลจากภายนอกไม่ได้และการกำหนดเส้นทางการเชื่อมต่อ (CORS) ที่ผิดของ Backend ทำให้ Frontend มีปัญหา
วิธีแก้: เข้าไปกำหนดและเพิ่มรายชื่อ Origin เช่น `.railway.app` ภายในโปรเจกต์ของ Backend ผ่านไฟล์ `server.js` พร้อมกับการเรียกใช้งานตาราง Environment จาก Provider แทนการ Hardcoded ของ Localhost

### 5.3 จะเลือกใช้ Docker หรือ Cloud เมื่อไหร่?

- ใช้ Docker เมื่อ: ทีมงานต้องการรันแอปพลิเคชันอย่างสะดวกรวดเร็วในสภาวะแวดล้อมแบบ Development/Testing พร้อมควบคุมซอฟต์แวร์ได้อย่างเบ็ดเสร็จตั้งแต่ฐานล่าง
- ใช้ Cloud (PaaS) เมื่อ: แอปพลิเคชันพร้อมปล่อยเข้าสู่สถานะ Production เพื่อมีผู้ใช้งานบริการจริง (Real-users) ที่ต้องการ Uptime ที่ดีที่สุด พร้อม Auto-scaling ที่ไม่ต้องเสียเวลาหรือบุคลากรเฝ้าเครื่องตลอด 24/7
