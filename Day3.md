# Day 3 (Deploy)

## API Documents
### [Swagger](https://swagger.io/) 
API Documents แบบ Dynamic 
- สามารถเขียนโค้ดแล้ว Generate API Document 
- สามารถเขียน API Document แล้ว Generate เป็นโค้ดก็ได้

### [API Blueprint](https://apiblueprint.org)
API Blueprint. A powerful high-level API description language for web APIs.
- มีการทำ Testing
- มีการทำ Mock data

## API Gateway
Concept คือมาตรงกลางแล้วแบ่ง request เป็น routes

### [Kong](https://konghq.com/kong/) 
The World’s Most Popular Open Source Microservice API Gateway and Platform.

## Cloud Native
- [Cloud Native Interactive Landscape](https://landscape.cncf.io/)

## DevOps
- ตารางธาตุ https://xebialabs.com/periodic-table-of-devops-tools/

## Service Discovery
- [Consul](https://www.consul.io/)

## Circuit breaker
ทุกภาษาจะมี Library เกี่ยวกับ Circuit breaker ให้อยู่แล้ว
  
### [circuit-breaker.js](https://github.com/yammer/circuit-breaker-js)
- เมื่อมันถูกเปิดขึ้น default มันจะปิดตัวเองภายใน 10วิ

## CQRS
- เกิดจากภาษาเชิง Object Oriented
- แบ่งกลุ่ม services เป็น Query Services (Read) กับ Command Services (Write)
- การแยกกันจะแยกการเก็บ Data ซึ่งพอมี Data หลายที่ มันจึงจำเป็นต้อง Sync กัน โดยให้ `Event Store`
- https://www.rabbitmq.com/

## Event Sourcing
- Immutable
- Time series
- Query ได้
- สามารถย้อนหลัง(Undo) ได้ด้วย
- มี Logs ประวัติข้อมูลทั้งหมด สามารถ Rebuild ทั้งหมดก็จะได้ข้อมูลเดิม
- เลือก Rollback กี่ step ก็ได้
- ทำให้การ Audit ทำได้ง่าย
- สามารถประยุกต์ tracing มาใช้ได้

### [Kafka](https://kafka.apache.org/)
ใช้เป็นตัวกลางในการเชื่อมต่อ เพื่อช่วยแยกการสื่อสารระหว่างระบบแต่ละตัว ไม่ว่าระบบอะไรต้องการข้อมูลจากไหน ก็มาเรียกใช้ใน Apache Kafka ตัวนี้ตัวเดียว
- consume สามารถรันทิ้งไว้หลาย services ได้เลย
- มีการกระจาย (distributed) การเก็บข้อมูลใน clusters
- มีความยืดหยุ่น (resilient architecture) เช่น มีการทำสำเนาข้อมูลซ้ำ (replication)
- มีการทนต่อความเสียหาย (fault tolerent)
- มีความสามารถในการขยายเชิงขนาน หรือ เพิ่มเครื่อง (node) ใน cluster ได้ (horizontal scalability)
- มีประสิทธิภาพด้านความเร็ว (latency น้อยกว่า 10ms)
- มีระบบที่ใหญ่ๆ ที่ใช้ Apache Kafka อยู่มาก เช่น Linkedin, Netflix, AirBnB, Yahoo, Wallmart หรือ LINE สวัสดีวันจันทร์นั่นเองของเรานั่นเองงง

### ESB
การติดต่อสื่อการกันผ่านตัวกลาง เอามาใช้ในเรื่องของการแชร์ data [Recreating ESB antipatterns with Kafka](https://www.thoughtworks.com/radar/techniques/recreating-esb-antipatterns-with-kafka)

## Monitoring
เป้าหมายเพื่อให้หาปัญหาได้อย่างรวดเร็ว
### [Prometheus](https://prometheus.io/)
- เป็น Time series database
- ข้อมูลจะถูกเก็บเป็น Key, Value (เก็บทีละนิด แต่เก็บบ่อยๆ)
- อยากดูตัวเลข Usage ต่างๆ
- มี Alert, Integrations
- เรียกข้อมูลจาก Services มาเก็บ

### [Grafana](https://grafana.com/)
- เอาข้อมูลจาก`Prometheus` มา Visualize data ให้สวยงาม 
- อะไรที่เราต้องการรู้ตลอดเวลา 
- Dashboard ที่ดูแล้วไม่เข้าใจทันที ถือว่าผิด
- Alert ให้ฉลาด ไม่ต้องเยอะ ไม่ต้องเมล

## Microservice Testing
Microservices ต้องเทสทุกอย่างอยู่แล้ว ควรทำให้เป็นกิจกรรมที่ต้องทำต่อเนื่องตลอดเวลา
- การ Testing แบบเดิม `Manual tests` -> `Automated GUI Tests` -> `Integration Tests` -> `Unit Tests`
- การ Testing ปัจจุบัน `Manual Session Based Testing` -> `Automated API Tests` -> `Automated Integration Tests` -> `Automated Component Tests` -> `Automated Unit Tests`  (แนะนำ)
- ก่อน Testing ควรคุยโครงสร้างกันก่อน
- `Unit Testing` คือเอา code ชุดหนึ่งมาทดสอบ แยก test แต่ละส่วน (แต่ส่วนใหญ่ Dev ชอบทำให้เสร็จทีเดียว 😅) 
- `Integration Testing` ทดสอบส่วนที่ต้องติดต่อระหว่าง ***External Service*** หรือ ***External Datastore***
- `Component Testing` ทดสอบภายใน ***Service*** ให้ทำงานได้ตามสถานการณ์ที่เรากำหนดไว้ ด้วยการจำลองสิ่งแวดล้อมและสถานการณ์รอบข้าง
- `Contract Testing` ทดสอบว่าใครเรียกใช้ Service ของเราบ้าง หรือเอา Contract ไปทดสอบกับ Service ที่เรียกว่าทำงานได้มั้ย??
- `End-to-End Testing` ทำ Manual Testing ทีละ Flow 

## Mock API Server
+ Stubby4j
+ WireMock
+ mbtest

## Notes
- ก่อนเริ่มต้นทำงานเราต้อง Design Contact Point ก่อน
- ยิ่งเป็น Microservices ยิ่งมีเอกสารเยอะ
- หนังสือ Domain-Driven Desing
- Monitoring ต้องไม่ผูกกับ Service ถ้าพังให้พังทีละอย่าง
- อย่าเชื่อใจ Data จากระบบอื่น
