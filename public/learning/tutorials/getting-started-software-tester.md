---
title: "Getting Started as a Software Tester"
tags: [learning, tutorial, testing]
created: 2026-02-26
---

2025/06/14
Miro board : sck.pub/ks-4-ts
# Software Tester
Software Tester ต้องรู้อะไรบ้าง สำหรับผู้เริ่มต้นในสายงานนี้[1](https://www.youtube.com/watch?v=SFQ94maoWuk)[2](https://www.youtube.com/watch?v=N5YMrQjyTfI)[3](https://www.youtube.com/watch?v=Yoi5k3q5XFA)[4](https://www.youtube.com/watch?v=0GJxBRwEmik)
General Software Test Process
General Software Test Life Cycle (STLC) ซึ่งเชื่อมโยงกับ Software Development Life Cycle (SDLC)
# General Software Test Process

## Product Backlog Refinement (Grooming)
## Test Development

### Plan
### Requirements
วิเคราะห์และระบุเงื่อนไขทางธุรกิจที่จะถูกทดสอบ
- ต้องมีองค์ความรู้ *หัวหน้างานมีหน้าที่ทำให้ลูกน้องเก่งเท่ากันหรือเก่งกว่า*
	- Business Domain
	- End-to-End Business Process
	- User Experience
- ต้องมี Soft Skills
	- Analytic Thinking
	- System Thinking
	- Logical Thinking
- เครื่องมือที่ใช้
	- Flow Chart
	- Swim Lane
	- Sequence Diagram
### Analysis & Design
วิเคราะห์และออกแบบ Test Scenarios
วิเคราะห์และออกแบบ Test Data
### Test Preparation
#### เขียน Test Case 
[1](https://www.youtube.com/watch?v=G9NtPYc2M6U)
#### เขียน Test Scenario
Test Scenario = สิ่งที่ต้องเตรียมการ + ข้อมูลในชีวิตจริงที่ใช้ในการทดสอบ + เงื่อนไขที่จะถูกทดสอบ + ผลการทดสอบที่คาดหวัง
#### เตรียม Test Data
ข้อมูลที่ใช้ต้องเหมือนข้อมูลที่ใช้ใน Production โดยมีการทำ Data Masking สำหรับ Sensitive Data
ต้องมี Technical Skill
- SQL
- Programming: JavaScript, TypeScript, Python
เครื่องมือที่ต้องรู้จัก
- Database ที่ Application ใช้งาน: Backup และ Restore
	- Structured Database: MySQL, PostgreSQL, Oracle
	- Unstructured Database: MongoDB, Cassandra
	- File based Database
- `Git` สำหรับบริหารจัดการ Source Code: Gitlab, Github, Bitbucket
	- เขียน Commit Message ให้ถูก patterns
	- ใช้ CLI เพื่อให้ตรวจสอบ Conflict ก่อน
- Docker, Docker Compose, K8S (Kubenetes)
- Terraform
#### เตรียมอุปกรณ์ที่ใช้ทดสอบ
ส่วนใหญ่ QA หรือ Tester ต้องเตรียมเอง
ใช้อุปกรณ์จริง หรือ Device Simulator
##### Web Application
###### Browser
ตามที่กำหนดไว้ในข้อตกลงตรวจรับงาน
ระบุ Version ของแต่ละ Browser 
###### Operating Systems (OS)
ตามที่กำหนดไว้ในข้อตกลงตรวจรับงาน
ระบุ Version ของแต่ละ OS 
###### Resolution
ความละเอียดของหน้าจอของ End User
###### Mouse or Trackpad
###### Internet Connection
ความเร็วในการใช้งาน
รูปแบบการใช้งาน
- LAN
- WiFi
- Mobile ต้องใช้ซิมให้ตรงกับ End User ที่ใช้งานจริง
การเข้าใช้งานผ่าน VPN หรือไม่
##### Mobile Application
###### Operation Systems (OS)
ตามที่กำหนดในข้อตกลงตรวจรับงาน
- Android
- iOS
- อื่นๆ เช่น Huawei Harmony OS
ระบุ Version ของแต่ละ OS ด้วย
Feature / Security / Permission อะไรบ้างที่เกี่ยวข้อง
###### Development Framework
พํฒนาแบบ Native, Cross-Platform หรือ Web View
###### Supporting Device
รองรับอุปกรณ์ยี่ห้อไหน รุ่นไหนบ้าง
ขนาดของหน้าจอ
## Test Execution
### Manual Test Execution

### Automate Test Execution
ต้องไม่มีคนเข้าไปเกี่ยวข้องในกระบวนการทดสอบ รายงาน ทดสอบซ้ำ
### Implementation
### Testing
#### Web Application
ทดสอบ Presentation Layer / Web UI ที่ Web Server
ความรู้ที่ต้องใช้
`Software Architecture`
- Three-Tier Architecture: Web UI, Business Logic, Data Stored
`HTML (HyperText Markup Language)` และ `DOM (Document Object Model)`
`CSS (Cascading Style Sheets)`
`JavaScript` 
`TypeScript`
- `Cypress` 
- `Playwright`
`Python`
- `Robot Framework` คู่กับ `Selenium Library`
`Java`
- `Kotlin Record & Playback`
Browser Developer Tools
X Path ของแต่ละ Browser
IDE
Library
Database
ระบบที่ไม่ควรทำ Automation
- Captcha / Email Verification / OTP Verification
	- Dev ต้อง Flag ถ้าเป็น Test Mode ให้ Bypass แล้วค่อยปิดบน Production
- Encoded Source Code ที่ต้อง Decrypt ก่อน
- Generate Code ที่มีการ Random ID
Environment Set Up
- Provision ระบบขึ้นมา
- ติดตั้ง Artifacts ที่จะถูกทดสอบ
- โครงสร้าง และข้อมูลใน Database
	- ถ้าไม่ควบคุม จะเกิด Freaky Test ที่ผลเชื่อถือไม่ได้
Infrastructure as a Code
ไม่ควรใช้ Public AI ในการทำงานจริง
- SIT และ UAT มักอยู่บน Private Network
- การนำ Source Code (ทรัพย์สินของบริษัท) ออกไป มีความผิด
- ใช้ในการฝึกประสบการณ์ได้ แต่ต้องทำความเข้าใจและตรวจสอบคำตอบด้วย
#### API Testing
ทดสอบที่ Business Logic (Application Server)
สิ่งที่ต้องรู้
- ต้องรู้ว่าสื่อสารกันด้วยภาษาอะไร รูปแบบไหน
	- SOAP เป็นรูปแบบ XML
	- REST
	- GraphQL
	- Socket
	- gRPC
- HTTP Protocol
- ต้องอ่าน API Spec เป็น
	- Method
	- Content Type
	- Header
	- Data Type โดยเฉพาะ Array
	- Global and Local Variable
	- Hidden Value
	- Token
	- Authentication
- เข้าใจ Synchronous และ Asynchronous
- ใช้ API Testing Tools ได้
	- Postman
	- Newman
### SIT
### UAT
### Log Defect
### Retest
[Requirement Change](https://www.youtube.com/watch?v=VcUjtPjhxuU)
### Regression Test
### Test Report
[n8n for test report](https://www.youtube.com/live/3AkaFZKjN2U)
# Performance Testing
## Core Activities
### Test Development
#### Identify test environment
#### Identify acceptance criteria
กำหนด ข้อตกลง ค่าที่ต้องวัด เพื่อใช้ในการตรวจรับงาน และจ่ายเงินอย่างสบายใจ
- Resource Utilization ที่สนใจ
	- CPU Utilization
	- Memory Utilization
	- I/O Utilization
#### Plan and design tests
รู้จัก Wordload Model แต่ละรูปแบบว่าจะทดสอบรูปแบบใด
รู้ว่าจะร่วงที่จุดไหน คนที่ออกแบบ Architecture ต้องตอบได้
#### Prepare test environment
#### Record test plan
## Test Execution
#### Run the tests
#### Analyze results
#### Performance Tuning / Change configuration