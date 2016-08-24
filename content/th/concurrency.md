## VIII. Concurrency
### เสกลออกผ่านทางโมเดลของโปรเซส

ไม่ว่าจะเป็นโปรแกรมคอมพิวเตอร์ใดๆ เมื่อรันแล้ว จะมีโปรเซสเกิดขึ้น อาจจะ 1 โปรเซสหรือมากกว่า 1 โปรเซสขึ้นไป เว็บแอพพลิเคชั่นมีรูปแบบการรันโปรเซสหลายรูปแบบ  ยกตัวอย่างเช่น โปรเซสของ PHP จะรันเป็นโปรเซสลูกของ Apache และจะเริ้มขึ้นเมื่อมีความต้องการจากปริมาณการรีเควส  โปรเซสของ Java จะใช้วิธีในทางตรงกันข้าม โดยที่ JVM จะเตรียมโปรเซสขยายใหญ่ 1 โปรเซสโดยที่โปรเซสนี้จะจองทรัพยากร(CPU และ หน่วยความจำ) จำนวนมากตั้งแต่เริ่มต้น, ในการจัดการ concurrency จะจัดการภายในโดย threads  ในทั้ง 2 กรณีที่กล่าวข้างต้นนี้ โปรเซสที่รันอยู่เป็นสิ่งที่เล็กที่สุดที่นักพัฒนาจะสามารถจับต้องและจัดการได้

![Scale is expressed as running processes, workload diversity is expressed as process types.](/images/process-types.png)

**ในแอพ twelve-factor นั้น โปรเซสถือว่าเป็นประชาชนชั้นหนึ่ง**  โปรเซสในแอพ twelve-factor  มีบทบาทอย่างมากใน [the unix process model for running service daemons](https://adam.herokuapp.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/).  ในการใช้โมเดลนี้ นักพัฒนาจะสามารถวางแผนแอพของพวกเขาให้รองรับ workloads หลากหลายรูปแบบโดยการมอบหมายชนิดของงานที่ทำไปยัง* ชนิดของโปรเซส* ที่เหมาะสม  ยกตัวอย่างเช่น, รีเควส HTTP สามารถมอบหมายให้เว็บโปรเซส และงานที่ต้องใช้เวลานานและทำในแบ็คกราวด์สามารถมอบหมายให้ worker โปรเซสจัดการได้

แต่ก็ไม่รวมรวมถึงโปรเซสที่เป็นเอกเทศจากจัดการตัวเองภายใน เช่นทาง threads ภายใน runtime VM, หรือ async/evented โมเดลใช้ในเครื่องมือเช่น [EventMachine](http://rubyeventmachine.com/), [Twisted](http://twistedmatrix.com/trac/) หรือ [Node.js](http://nodejs.org/)  เนื่องจาก VM ที่เป็นเอกเทศสามารถขยายจนมีขนาดใหญ่ (vertical scale) ดังนั้นแอพพลิเคชั่นจะต้องสามารถแบ่งกระจายงานไปยังโปรเซสหลายๆตัวที่อยู่บน physical machine หลายเครื่องได้

โมเดลของโปรเซสแบบนี้จะแสดงให้เห็นประสิทธิภาพอย่างมากเมื่อถึงเวลาที่จะต้องสเกลออก  [share-nothing, horizontally partitionable nature of twelve-factor app processes](./processes) หมายความว่าเมื่อต้องการเพิ่ม concurrency จะสามารถทำได้ง่ายและเชื่อถือได้ ชุดของชนิดของโปรเซสและจำนวนของโปรเซสในแต่ละชนิด เรียกว่า *รูปแบบโปรเซส (process formation)*.

โปรเซสของแอพ Twelve-factor [จะไม่เป็น daemonize](http://dustin.github.com/2010/02/28/running-processes.html) หรือสร้างไฟล์ PID  ในทางตรงกันข้ามจะขึ้นอยู่กับระบบจัดการโปรเซสของระบบปฏิบัติการ (ตัวอย่างเช่น [Upstart](http://upstart.ubuntu.com/), ระบบจัดการโปรเซสแบบกระจาย(distributed process manager) บนคลาว์ด หรือเครื่องมือเช่น [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) ใน development) สำหรับจัดการ [output streams](./logs), ตอบสนองต่อโปรเซสที่ค้าง และรอรับคำสั่งจาก user ที่จะ restart และ shutdown
