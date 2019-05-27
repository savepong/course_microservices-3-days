# Day 1

### Cloud Native
- ธุรกิจต้องการการส่งมอบอย่างต่อเนื่อง
- DevOps เป็นวัฒนธรรมองค์กร
- UAT ต้องใช้ user จริงทำ และห้ามบอกว่าคลิกตรงไหน
- Microservice มีข้อเสียถ้าไม่เข้าใจหลักการ

### ปัญหาของระบบแบบ Monolith
- ตรงกลางล่มตายหมด
- ส่วนใหญ่จะแบ่งเป็น layers เช่น Frontend, Backend, Database
- ระบบ Legacy จะแก้ไขยาก กระทบหลายส่วน
- ปัญหามักถูกดองไว้ เพราะทีมต้องการแค่รันให้ทันเวลา
- แต่ละ Module ไม่แยกเป็นอิสระจากกัน 
- มี Common Module ที่แชร์กันไปเยอะ ทำให้เวลาแก้แล้วกระทบหลายส่วน
- ใน Database มี tables เยอะเกิน แต่ละ table มี fields เยอะเกิน (เคยมีตัวอย่าง 3,000 fields เผื่อไว้เติม field ไม่ต้องการ alter เพราะมันเป็น cost)
- คนพัฒนามีอายุการทำงานโค้ดน้อย เพราะเติบโตไปทำส่วนอื่น และเน้นประชุมเยอะขึ้น
- บางครั้งปัญหาเกิดจากการ Update Firmware ของ Database
- Do all modules need to use the same Database, Language and Runtime .. ?

### SOA (Service-Oriented Architecture)
- แนวคิดคือส่งมอบ Software ที่ดีอย่างยั่งยืน http://soa-manifesto.org
- Focus ที่ Business value ไม่ใช่เทคนิค
- Focus ที่ความยืดหยุ่นมากกว่า
- ออกแบบ Architecture ให้อยู่ได้นานที่สุด (แต่มันไม่มีจริงหรอก 😂)
- Architecture ที่ดีสามารถปรับปรุงได้อย่างต่อเนื่อง
- Shared service อะไรที่เหมือนกันจะเอามารวมกัน
- มี Organize องค์กรที่ซับซ้อน
- Business service ไม่สามารถ Reuse ได้
- Enterprise service จะมีขนาดใหญ่ ต้องใช้ Application service
- Application service ต้องพึ่ง Inflastucture service
- เกิดตำแหน่ง Project Manager เพื่อมาจัดการ Coordination across all teams (Cost เพิ่มอีกแล้ว)
- มี layer หลายชั้นกว่าจะถึง core service (หัวหอม architecture 😅)

### แนวคิด Service ที่ดีที่สุด
- Standalone อยู่ได้ด้วยตัวเอง
- Indipendence ต้องอิสระ
- ให้ Value ทั้ง Business และ IT
- ลด Service ตัวกลาง (Service1 -> Service2 -> Service3)

### Microservice
- คือระบบแบบกระจาย `Distributed services` ทำให้เรากล้าที่จะลองของบางอย่าง ไม่กระทบทั้งหมด 
- มี Services 2 แบบ คือ `Business service` และ `Infrastructure service` ดูแลโดย Application Development Team
- Focus ที่ `Business service` เพื่อดำเนินธุรกิจ แต่ต้องมี `Infrastructure` ที่นิ่ง
- ควรจะ Minimal coordination ลดการจัดการลง (ลด cost ได้ด้วย 😃)
- การปรับใช้ `Microservice` แทน `Monolith` ควรจะทะยอยทำทีละ Service หรือทีะ Module
- ไม่ควรให้มี Data conversion ที่มีการทำอะไรบางอย่าง support ให้กับแค่บาง service ที่มี customize พิเศษ
- Shared service ต้องเป็น global common service จริงๆ เท่านั้น!!!
- มี common ที่เป็น local ของ service นั้นๆ
- Deploy ง่าย (อย่าไปเชื่อ แม่งยากมาก)
- ถ้า service เดิมมีการแบบ Major change ควรสร้าง service ใหม่ไปเลย
- การเปลี่ยนมาเป็น `Microservice` เราต้องคุมกำเนิด `Monolith`
- อย่าใช้ Database ที่เดียวกันทั้งหมด
- ต้องมี Small team ที่คอยดูแลแต่ละ service สามารถจบงานด้วยทีมตัวเองได้
- ควรจะตรวจสอบได้ว่าแต่ละ Request วิ่งไปที่ service ไหนบ้าง
- ควรตอบได้ว่าแต่ละ service มีการใช้ success rate เท่าไหร่ fail rate เท่าไหร่
- มี logs เก็บทุกการ change
- Microservices ต้องการคนหลายกลุ่มมาช่วยกันออกแบบ
- https://microservices.io

### Drawbacks of Microservice (ข้อเสีย)
- การเรียก service ไม่ควรเรียกลึกเกิน 3 services (Core -> Service1 -> Service2 -> Service3)
- services ในองค์กรมันจะเป็นแบบ graph services การจัดการทำได้ยุ่งยาก

### ** Notes
- Architecture ที่ดีที่สุดอยู่บน Production นั่นแหละ!!
- ยิ่งมี standard เยอะยิ่ง cost เยอะ
- การโอนเงินสำหรับธนาคารเป็น Infra service เพราะเป็น core
- Netflix ใช้ AWS เป็นหลัก (ทำ AWS ล่มในช่วงแรก)
- Service มันคือ Service ถ้าเรา custom ให้ลูกค้ามันก็ไม่ใช่ Service !!!
- ระบบ Network มักล่มได้ตลอดเวลา (อย่าไว้ใจ)
- วิธีจัดการกับความพังด้วยการทำให้มันพังเป็นปกติ 🤣
- ระบบ Network มักล่มได้ตลอดเวลา (อย่าไว้ใจ)
- เลือก DB ให้เหมาะกับงานนั้นๆ (ไม่ใช่เอะอะๆ ก็ RMDB)
- งานงอกจะไม่มีถ้าเราคุยกัน :)
- Yelp เป็นระบบเว็บสำหรับการ review https://github.com/yelp/service-principles
- หาความซับซ้อนให้เจอ แก้ให้ตรงจุด
- องค์กรควรทำ services dependency (Blue print) วาดภาพออกมาแล้วต้องเป็น Tree ไม่ควรเป็น Graph
- Logs ต่างๆ ที่เราเก็บแยกกันควรทำให้เอามาใช้ประโยชน์
- ส่ิงที่แปลกประหลาดมากคือ เรามักจะไป maintain ของที่ไม่ได้ใช้
- ถ้าระบบเดิมไม่มีปัญหา ก็ไม่ต้องใช้ Microservices เพราะการแยกบางทีก็ทำให้เกิดปัญหา
