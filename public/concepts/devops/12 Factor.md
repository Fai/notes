[The Twelve-Factor App](https://12factor.net/)

**Mindset to build scalable apps**
1. Codebase
    - แต่ละ service มี codebase เป็นของตัวเอง
2. Dependencies
    > Explicit declare and isolate dependencies
    - ระบุเวอร์ชั่นของ dependency
    - isolate environment
        - virtual environment
        - docker container
3. Config
    > store config in environment variable
    - สามารถ open source ได้โดยไม่เปิดเผยข้อมูล sensitive
4. Process
    > stateless and share nothing
5. Build, Release and Run
    > use strictly separation between build, release and run stages
    - build ด้วย docker build
    - release ด้วย docker image + config file
    - run บน environment ที่หลากหลายได้
6. Backing Services
7. Port Binding
    > is completely self-contained.
8. Concurrency
9. Disposability
    > The twelve-factor app’s processes are disposable, meaning they can be started or stopped at moment’s notice. The twelve-factor app’s processes should shutdown gracefully when they receive a SIGTERM signal from the process manager.
    - SIGTERM แล้วหยุดรับ req ทำงานให้เรียบร้อยก่อนปิด
    - SIGKILL ถ้าหมดเวลาแล้วยังไม่ปิด
10. Dev Prod Parity
    > The twelve-factor app is designed for continuous deployment by keeping the gap between and production small. The twelve-factor developer resists the urge to use different backing services between development and production
    - ระหว่าง Dev กับ Ops จะมี gaps ไม่ว่าจะเป็นเรื่อง Time , Tools
11. Logs
    > The twelve-factor app never concerns itself with routing or storage of its output stream. Store logs in a centralized location in a structured format.
    - ใช้ centralized log management เช่น ELK stack, fluentd, splunk, datadog