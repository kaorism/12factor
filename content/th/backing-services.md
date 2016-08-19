## IV. Backing services
### จัดการเซอร์วิสเบื้องหลังเหมือนกับทรัพยากรที่นำมาติดเพิ่มเติม

เซอร์วิสเบื้องหลัง *backing service* คือเซอร์วิสใดๆ ที่แอพเรียกใช้งานผ่านเนตเวิร์ก และเป็นส่วนหนึ่งในการทำงานหลักของแอพ.  ตัวอย่างรวมถึงการเก็บข้อมู
 (เช่น [MySQL](http://dev.mysql.com/) หรือ [CouchDB](http://couchdb.apache.org/)), ระบบ messaging/queueing  (เช่น [RabbitMQ](http://www.rabbitmq.com/) หรือ [Beanstalkd](http://kr.github.com/beanstalkd/)), ระบบ SMTP สำหรับส่งอีเมล (เช่น [Postfix](http://www.postfix.org/)), และ ระบบแคช (เช่น [Memcached](http://memcached.org/)).

เซอร์วิสเบื้องหลัง ตัวอย่างเช่น database แต่ดั้งเดิมนั้นจะจัดการโดยผู้จัดการระบบคนเดียวกันกับผู้ที่ติดตั้งแอพ  นอกเหนือจากนี้การที่จัดการแบบเซอร์วิสแบบ จัดการภายใน (locally-managed) และนอกจากนี้แอพอาจจะมีเซอร์วิสอื่นๆ ที่ใช้บริการจาก third parties อีกด้วย  เช่น เซอร์วิส SMTP (เช่น [Postmark](http://postmarkapp.com/)), เซอร์วิส metrics-gathering (เช่น [New Relic](http://newrelic.com/) หรือ [Loggly](http://www.loggly.com/)) เซอร์วิส binary asset  (เช่น [Amazon S3](http://aws.amazon.com/s3/)) และแม้กระทั่ง เซอร์วิส API-accessible consumer (เช่น [Twitter](http://dev.twitter.com/), [Google Maps](http://code.google.com/apis/maps/index.html), หรือ [Last.fm](http://www.last.fm/api)).

**โค้ดของแอพ twelve-factor จะไม่มีความแตกต่างระหว่างเซอร์วิสภายในกับเซอร์วิสจาก third party **  สำหรับแอพนั้น ทั้ง 2 เซอร์วิสคือทรัพยากรที่นำมาติดเพิ่ม, เข้าถึงการใช้งานผ่านทาง URL หรือ locator/credentials ที่จัดเก็บไว้ใน [คอนฟิก](./config).   [การติดตั้ง](./codebase) ของแอพ twelve-factor จะต้องสามารถเปลี่ยนจากฐานข้อมูล MySQL ภายใน local เป็น MySQL ที่จัดการโดย third party (such as [Amazon RDS](http://aws.amazon.com/rds/)) โดยไม่จำเป็นต้องแก้ไขโค้ดใดๆ หรือเหมือนกับ เปลี่ยน  SMTP server ภายใน local เป็นการใช้ third-party เซอร์วิส SMTP  (เช่น Postmark) โดยไม่จำเป็นต้องแก้ไขโค้ดใดๆ  ในทั้ง 2 กรณีนี้ ทำเพียงแค่แก้ไขการจัดการทรัพยากรในคอนฟิกเท่านั้น

ในแต่ละเซอร์วิสเบื้องหลังคือ *ทรัพยากร(resource)*  ยกตัวอย่างเช่น ฐานข้อมูล MySQL คือทรัพยากร; ฐานข้อมูล MySQL 2 ฐานข้อมูล (ใช้สำหรับ sharding ใน เลเยอร์แอพพลิเคชั่น) ถือว่าเป็นทรัพยากร  2  ทรัพยากรที่ต่างกัน  แอพ twelve-factor จะถือว่าฐานข้อมูลเหล่านี้ databases คือ *ทรัพยากรที่นำมาติด (attached resources)* ซึ่งเป็นตัวชี้วัดถึงความความเป็นอิสระออกจากกันระหว่างการติดตั้งกับทรัพยากรที่นำมาติดกับแอพ

<img src="/images/attached-resources.png" class="full" alt="การติดตั้ง production โดยติดต่อกับเซอร์วิสเบื้องหลัง 4 เซอร์วิส" />

ทรัพยากรสามารถนำมาติดและถอดออกจากการติดตั้งได้ตามต้องการ ยกตัวอย่างเช่น ถ้าฐานข้อมูลของแอพนั้นทำงานผิดปกติเนื่องจากปัญหาฮาร์ดแวร์ ผู้ดูแลระบบสามารถสร้างฐานข้อมูลใหม่จากข้อมูลที่ backup  แล้วต่อจากนั้นสามารถถอดฐานข้อมูลปัจจุบันออกและเปลี่ยนเป็นฐานข้อมูลใหม่ที่สร้างขึ้นโดยไม่ต้องแก้ไขโค้ดใดๆ

