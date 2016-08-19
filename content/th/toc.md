The Twelve Factors
==================

## [I. Codebase](./codebase)
### โค้ดเบสเดียวสามารถแทร็กได้จากระบบความคุมเวอร์ชั่น (revision control), ติดตั้งได้หลายครั้ง

## [II. Dependencies](./dependencies)
### ส่วนที่ขึ้นต่อกัน (dependencies) สามารถประกาศได้จากภายนอกและแยกจากกัน

## [III. Config](./config)
### เก็บคอนฟิกไว้ที่ environment

## [IV. Backing services](./backing-services)
### จัดการระบบหลังเหมือนกับทรัพยากรที่นำมาติดเพิ่มเติม

## [V. Build, release, run](./build-release-run)
### แยกส่วนระหว่างขั้นตอน build และ run อย่างชัดเจน

## [VI. Processes](./processes)
### ประมวลผลแอพพลิเคชั่นแบบเดียวกับ โปรเซสที่ไม่มีสเตต (stateless) หนึ่งโปรเซสหรือหลายๆโปรเซส

## [VII. Port binding](./port-binding)
### เปิดการเชื่อมต่อกับเซอร์วิสภายนอกผ่านทางพอร์ท

## [VIII. Concurrency](./concurrency)
### เสกลออกผ่านทางรูปแบบโปรเซส

## [IX. Disposability](./disposability)
### เพิ่มความทนทาน (robustness) โดยการเริ่มต้นใหม่เร็วและปิดตัว (shutdown) อย่างสมบูรณ์

## [X. Dev/prod parity](./dev-prod-parity)
### ทำให้ development,  staging,  production คล้ายกันให้มากที่สุด

## [XI. Logs](./logs)
### จัดการล็อกแบบอีเวนท์สตรีม (event stream)

## [XII. Admin processes](./admin-processes)
### รันงาน แอดมิน/จัดการ ด้วยโปรเซสที่รันครั้งเดียวเมื่อต้องการใช้ (one-off process)
