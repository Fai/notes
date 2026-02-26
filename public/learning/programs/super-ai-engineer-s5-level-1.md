---
title: "Super AI Engineer Season 5 — Level 1"
tags: [learning, program, ai]
created: 2026-02-26
---

# Super AI Engineer Season 5 — Level 1

# Theory Assessment

## AI Ethics
- Diversity พัฒนาให้เกิดประโยชน์ต่อผู้คนมากเท่าที่ทำได้ คำนึงถึงความหลากหลาย หลีกเลี่ยงการผูกขาดแบ่งแยกและเอนเอียง
- Fairness พิสูจน์ความเป็นธรรมได้
- Competitiveness and Sustainability Development ควรวิจัยและพัฒนาอย่างต่อเนื่อง และใช้งานเพื่อเพิ่มความสามารถในการแข่งขันและสร้างความเจริญ อย่างเป็นธรรม และยั่งยืน
## Python Programming
- ไม่ใช่ natural language
- `int(a)` ใช้แปลงเป็น int
- Pandas : manipulate DataFrame for Exploratory Data Analysis and Visualization
## Linux
- `kill` send signal to kill specific process by PID
- `pkill` send signal to kill specific process by name
- `killall` send signal to kill all running specific process
- `chmod` มีผลต่อสิทธิเข้าถึงไฟล์
## Kaggle
- support multiple `R` and `Python` session
- session must be ready to run code
- magic function ทำงานเฉพาะใน code cell
- Settings ตั้งค่า Accelerator, Internet, ภาษา
- Team Competition ส่งคำขอร่วมทีมกับคนอื่นได้ จากเริ่มชื่อตัวเองก็เปลี่ยนชื่อทีมได้
- Public Leaderboard แสดงผลตลอด Private แสดงหลังจบ
## Colab
- connect runtime to use session storage (temporary)
- ทำงานบนระบบ cloud ต่างจาก jupyter notebook ที่ทำงานบนเครื่องผู้ใช้
- form field ใช้ป้อนข้อมูล โดยไม่ต้องแก้ไข code
- `cell` สร้างได้เรื่อยๆ
- `text cell` ใส่รูปภาพได้
- `code cell` ใส่ form ได้
- `!chmod 600 ~/.kaggle/kaggle.json` ใช้แก้ไข permission
- `drive.mount('/content/drive')'` เชื่อมต่อ google drive
## Github
- use `git checkout -b branchname` to create branch

## Data Science and Big Data
Data Science คือการค้นหาองค์ความรู้จากข้อมูล ต้องรู้
Data Scientist ช่วย Domain Expert หา Insight จาก Data ด้วย Computational Tools
Data Science 
- Business Intelligence
- Math and Statistics
- Computer Science
Data Product Sprint
- Data Scientist และ Data Engineer ร่วมกันทำความเข้าใจข้อมูล
Data Analysis
- Data Cleaning เตรียมข้อมูล
- Data Mining ค้นหา pattern
- Forecasting พยากรณ์
Data Business Model
- Information-based differentiation
- Information-based delivery network
- Information-based brokering
Data Business Model Value Chain
- Big data information flow
- Service use
- Software tools and algorithm transfer
Data Business Model Value Chain Components
- Big Data Framework / Application Provider
- Data Consumer, Data Provider
- System Orchestrator
Big Data
- Volume
- Velocity
- Variety
Type
- JSON อ่านเขียนได้ ใช้พื้นที่น้อย เข้าใจง่าย รองรับหลายภาษา เหมาะกับส่งข้อมูลผ่านเวบ
Data Base
- SQL: Relational Database for Structure Data (Tabular)
	- RDBMS : MySQL, postgreSQL
- NoSQL: Non-Relational Database for Unstructured Data 
	- Document : MongoDB, CouchDB
	- Key-value
		- Tabular: Cassandra, Hbase
		- Cache: Memcached, Coherence
	- Graph : neo4j, OrientDB
	- Wide-column Stores
SQL
- `SELECT` เลือก Fields ที่ต้องการ
- `FROM` ระบุตารางที่ดึงข้อมูล
- `WHERE` กำหนดเงื่อนไข
- `ORDER BY` เรียงลำดับผล โดย `DESC` เรียงจากมากไปน้อย
- `LIMIT` จำกัดจำนวนผลของคำสั่ง
Open Data
- Open Data คือข้อมูลเปิดที่ทุกคนนำไปใช้ได้โดยอินสระ
- Open Data เป็นส่วนนึงของ Publicly available data
- Open Government Data คือข้อมูลเปิดจากรัฐบาล
	- `data.go.th`
GeoSpatial Data ใช้ทำงานกับตำแหน่งที่ตั้งของข้อมูลต่างๆบนพื้นโลก
## Data Visualization
Principles for Effective Graphical Display
- Avoid distorting
- Induce the viewer
- Present many numbers in a small space
ความสำคัญ
- ช่วยในการตัดสินใจ
- ช่วยให้ได้รับข้อมูลถูกต้องเหมือนกัน
- ช่วยให้เห็นรูปแบบข้อมูลที่ซับซ้อน
Five Laws of Data-Ink
- Above all else, show the data
- Maximize the Data-Ink Ratio
- Erase non-data-ink
- Erase redundant data-ink
- Revise and Edit
Data Discretization แปลง Continuous attribute เป็น Discrete attribute
ประเภทของการนำเสนอ
- Pie Chart ไม่เหมาะกับข้อมูลแบบ Trends
- 100% Stack Bar Plot เหมาะกับการเปรียบเทียบสัดส่วน
- Category Comparison เหมาะกับข้อมูลที่มีการเปลี่ยนเทียบ

## Mathematics of Artificial Intelligence
- For All, For Some
- Closed-form expression เป็นสมการคณิตศาสตร์ที่แน่นอน คำนวณได้โดยตรง
- ในขณะที่โมเดลบางประเภท จะมี uncertainty เช่น
	- None Closed-form expression ที่ไม่มีสูตรสำเร็จ
	- Physics Model
	- Statisticial Model
## Classical Machine Learning
- Type
	- Supervised
		- Regression ทำนาย เลข 
		- Classification ทำนาย class
		- Random Forest เป็น ensemble model
			- ใช้ Scikit-Learn ในการสร้าง
			- ใช้ Bagging Method
	- Semi-supervised
	- Unsupervised
		- Clustering จัดกลุ่ม
	- Reinforcement Learning
	- Transfer Learning ส่งต่อองค์ความรู้ระหว่าง model 2 model ที่แก้ไขปัญหาที่แตกต่างแต่เกี่ยวข้องกัน
		- Pre-training บนข้อมูลขนาดใหญ่
		- Post-training (Fine-tuning) บนชุดข้อมูลเป้าหมายเฉพาะทาง
- Exploration มากเกินไป จะทำให้ Algorithm พลาด solution ที่ดีที่สุด แต่ exploitation มากเกินไป ก็จะทำให้ติดอยู่ในผลลัพธ์ท้องถิ่น
- PCA create new uncorrelated feature, reduce noises and data dimensions
- Singular Value Decomposition (SVD) 
	- แยกส่วนที่สำคัญของข้อมูลออกจากกัน (แยก matrix เป็นผลคูณ)
	- ใช้วิเคราะห์ข้อมูล บีบอัดข้อมูล ลดมิติ คำนวณ pseudo inverse ได้
	- ใช้เวลานานกว่า QR decomposition
- Max Pooling ใช้ลดขนาดของ input โดยเลือกค่ามากสุดในแต่ละกลุ่มของข้อมูล
- Bayesian Inference ช่่วยในการปรับปรุงความน่าจะเป็นของเหตุการณ์ตามข้อมูลใหม่
- Object Detection : SSD, YOLO, R-CNN
- Image Classification : EfficientNet
- Regression Model 
	- วัดผลด้วย R Square ใกล้ 1 แสดงว่า Fit กับ Data ได้ดี
	- วัดผลด้วย RMSE ดูว่าค่าทำนายจะเบี่ยงเบนเท่าไหร่โดยเฉลี่ย
-  Bias มาก แสดงว่า Underfit ต้องเพิ่ม Model Complexity
- แต่ยิ่ง Model Complexity สูง ยิ่งมี Variance สูง
- Validation set ใช้ในการปรับค่า Hyperparameters
- Histogram Equalization สามารถ Flatten Histogram ได้มากที่สุด
- L1 และ L2 Loss Function ใ่ช้ในการบอกทิศทางปรับหาค่าพารามิเตอร์ที่เหมาะสม จาก gradient ในการ optimize ซึ่งค่าความคาดเคลื่อนสามารถติดลบ
- Analysis of Variance (ANOVA)   
	- H0 ถูกปฏิเสธ แสดงว่า ค่าเฉลี่ยกลุ่มนึง ต่างจากกลุ่มอื่น
- Confusion Matrix
	- ถ้า False Positive สูง False Negative ต่ำ ระบบ Sensitivity สูง ให้ลดความไว
## Genetic Algorithm
- Partially mapped cross over (PMX) ช่วยรักษาความคล้ายคลึงของ ตำแหน่งสัมพัทธ์
- Selection Pressure เร่งกระบวนการและเพิ่มความหลากหลาย เพื่อหาคำตอบที่เหมาะสมที่สุด
##  Logical Agent
- รูปแบบหนึ่งของ rule-based ที่ใช้ first-order logic/propositional logic ในการอธิบาย  
## Neural Network and Deep Learning
- To use pre-trained model need same preprocessing and parameter freeze
- Multilayer Perceptron (MLP) เป็น Deep Learning Technique
- Perceptron แก้ได้เฉพาะปัญหาเชิงเส้น
- จำนวน parameters เท่ากับ feature
- เมื่อ Input เป็นลบ LeakyReLu ให้ output ความชันขนาดเล็ก ในขณะที่ ReLu ความชันเป็น 0 (เกิด dead neuron หยุดเรียนรู้)
## Natural Language Processing (NLP)
- Application
	- `Sentiment Analysis` determine attitude of speaker to topic or context 
	- `Language Identification`
	- `Readability Assessment`
	- `Genre classification` แบ่งประเภทตามลักษณะของเนื้อหา
- ระดับปัญหาทางภาษาศาสตร์
	- Morphology วิเคราะห์สัณฐานคำ
		- Word Segmentation การระบุขอบเขตหรือการตัดคำ
	- Syntax วิเคราะห์โครงสร้างไวยากรณ์
	- Semantic วิเคราะห์ความหมาย
	- Discourse วิเคราะห์ความเชื่อมโยงประโยค
	- Pragmatics วิเคราะห์ความหมายแฝง จากความรู้พื้นฐานทางโลก และความรู้ร่วมกันของคู่สนทนา
- Writing System use sets of symbols to represent the sounds of speech, punctuation and numerals
	- Abugidas ไทย พม่า เขมร ลาว
		- ภาษาไทยเป็นภาษาคำโดด analytic language เพราะหน่วยความหมายขนาดเล็ก ต้องใช้คำขยาย ลำดับตายตัว
	- Abjad อารบิก ฮิบรู
	- Alphabets โรมัน อาร์มีเนีย กรีก อังกฤษ เกาหลี
	- Syllabary ญี่ปุ่น(ฮิรางานะ)
- Lexitron เป็น Dictionary
- ตัวอย่าง corpus ภาษาไทย
	- norvig ngrams
	- arts chula
	- lexitron nectec
	- longdo
	- kobkrit nlp thai
	- nlp for thai
- Stop Word Removal
	- ลบคำสรรพนามที่บ่งบอกความสุภาพ
	- ลบคำลงท้ายที่แสดงอารมณ์
	- ลบคำบุพบทที่ไม่สำคัญ
	- ต้องระวังเรื่องการลบคำที่มีความสำคัญ เช่น ไม่
- ตัดแบบ Stemming ไม่สนความหมาย แต่ Lemma สน
- Text Analysis
	- Bag of words นับคำ ทำง่าย
	- Term Frequency-Inverse Domain Frequency (TF-IDF) เน้น Noun สำคัญ
- Text Normalization แปลงตัวเลขและตัวอักษรย่อเป็นคำอ่าน
- Word Tokenization แบ่งข้อความเป็นส่วนย่อย
	- Markov Model จะอิงความน่าจะเป็นจากคำก่อนหน้า
- Grapheme-to-Phoneme conversion แปลงเป็นเสียงอ่าน
- Sentence segmentation ตัดประโยค
- Part-of-Tagging มาก 
	- เหมาะกับระบบสังเคราะห์เสียง
	- Hidden Markov Model (HMM) จำลองสถิติแทน“ลำดับ” หรือ "เวลา" ของข้อมูล
- Word Embedding  แปลงคำเป็น vector ที่ dimension เท่ากัน
- Summarization
	- Single Document Summarization สกัดเอกสารฉบับเดียว 
	- Multiple Document Summarization สกัดเอกสารหลายฉบับที่พูดเรื่องเดียวกัน
- Topic Modeling ด้วย Laten Dirichlet Allocation มี distribution หลายประเภท
	- word distribution for a topic
	- word distribution for a document จะ observed ได้ทันที
	- per-topic word distributions
	- topic distribution for a document
- Name Entity Recognition
	- การแพทย์ : ชื่อยา ชื่อโรค ข้อมูลผู้ป่วย
	- องค์กร : company, ethic group, sport organization, government
## Transformer
- มีความซับซ้อนสูง ใช้ทรัพยากรมาก
- Word Similarity คำที่ความหมายใกล้เคียงกัน ค่าจะใกล้เคียง 1
	- Pointwise Mutual Information (PMI) วัดการเกิดร่วม
	- Euclidean Distance วัดระยะตรงระหว่างจุดของ vector
	- Cosine Similarity วัดมุมระหว่าง vector
- BERT (Bidirectional Encoder Representations from Transformers)
	- ประกอบด้วย Attention Model และ Encoder 
	- พัฒนาโดย Google AI สำหรับงาน NLP
	- สนับสนุนการประมวลผลข้อความ 2 ทิศทาง ไม่ว่าภาษาอะไร
- WangchanBerta ใช้สถาปัตยกรรม RoBERTa พัฒนาโดย AIResearch Thailand ร่วมกับ VISTEC และ DEPA
- Seq2Seq Model with Attention
	- แปลง input เป็น word embedding
	- ใช้ LSTM อ่าน input จากซ้ายไปขวา ขวาไปซ้าย เพื่อสร้าง Encoder Hidden State
	- Initial Decoder hidden state โดยเฉลี่ยค่าจาก Encoder
	- หา Attention Score และสร้าง context vector
	- Concatenate encoder hidden state
	- calculate target word distribution
## Machine Translation
- Encoder-Decoder
- Direct Machine Translation แปลจากต้นทางไปปลายทาง โดยไม่ใช้ตัวกลาง เช่น ไทย-ลาว
- Transfer Based Machine Translation วิเคราะห์โครงสร้างทางภาษาด้วย
- Example Machine Translation ใช้ฐานข้อมูลของคู่ประโยคมาเป็นตัวอย่างในการแปลใหม่
- Neural Machine Translation ใช้ Seq2Seq ที่มี Encoder-Decoder เรียนรู้และแปลภาษา
	- ต้องดึงข้อมูลจากแหล่งต่างๆ มาแปลงในรูปแบบ Parallel Sentence Pair
	- ทำความสะอาดข้อมูล ลบบรรทัดว่าง และข้อมูลซ้ำซ้อน
	- Tokenize ตัดคำ lowercase
	- แบ่งข้อมูลเป็น Train Dev Test
	- สร้าง Subword Units model ด้วย Byte-pair encoding
	- นำไป apply กับข้อมูลพร้อมสร้าง dictionary
- BLEU Score จำเป็นต้องคำนวณ Cumulative n-gram
## Signal Processing
- Signal in Medical
	- ECG (Electrocardiogram) สัญญาณชีพของหัวใจ
	- EEG สมอง
	- EMG กล้ามเนื้อ
	- EGG กระเพาะอาหาร
- Spectrogram = 2D spectrum (Time-Frequency)
- Formant = ความถี่ที่มีพลังงานสะสมสูงในเสียงพูด (resonant frequencies)
- Sampling Frequency = ความถี่ของการสุ่มจับสัญญาณที่ต้องการ
- Localization ของ Short Time Fourier Transform (STFT) เลือกช่วงสัญญาณแคบ ความถี่สูง ได้ time resolution ดี
- Digital signal (discrete) vs analog signal (continuous)
- Periodic Signal คือเกิดซ้ำรูปแบบคงที่ ถ้าไม่เรียก Nonperiodic
- Fourier Transform เป็น Continuous, Nonperiodic
- Fourier Seires คือการแทนสัญญาณ Periodic ด้วยสมการคณิตศาสตร์
## Speech
- Speech Recognition เกิดครั้งแรกชื่อ Audrey System (Automatic Digit Recognizer) ปี 1952
- Input sequence แบบ Phoneme สร้างระบบสังเคราะห์เสียงได้ถูกต้องและเป็นธรรมชาติ
- Process : Signal Level -> Acoustic Level -> Language Level
- ASR แบ่ง Language Level ออกเป็น
	- Language Model (LM) กำหนดลำดับการเชื่อมต่อของคำ
	- Pronunciation Dictionary เชื่อมโยงคำกับลำดับเสียง (phoneme)
- TTS (Text-to-Speech) แปลงข้อความเป็นเสียง Mel-to-spectogram
	- FastSpeech
	- FastPitch ใช้ Duration, Energy, Pitch
	- MixerTTS
## Image Processing
- Resolution คือ ความละเอียดของภาพ
- CMYK : เหมาะกับงานพิมพ์
- ข้อมูลที่ใช้ในการสร้างเป็นภาพได้
	- uint8 เก็บข้อมูล 0-255
	- float16
	- float32
	- float64
- Image Enhancement ทำให้ภาพคมชัด เน้นรายละเอียดในภาพ เป็นขั้นตอนแรก
	- Laplacian เป็น high-pass filter ใช้ sharpening
- Averaging filter กับ Gaussian Filter เป็น low-pass filter ทำให้เบลอ ลด noise
- Arithmetic Operations รวมภาพเข้าด้วยกัน
- Histogram Equalization ปรับความสว่างของภาพ
- Hu moments สกัดคุณลักษณะรูปร่าง
- Local Binary Patters : extract texture feature
- Harris Corner คัดเลือกลักษณะของวัตถุภายในภาพที่คล้ายมุมได้
- Vectorization ทำให้ Fully-connected layer ประมวลผลข้อมูลรูปภาพได้
- OpenCV
	- เปิดวีดีโอจาก File, IP Camera, Webcam
	- `cv2.imread()` : read image
- 3D Global Descriptors : ESF, VFH
- 3D Local Descriptors : NARF, SHOT, PFH
- Sampling กำหนดตำแหน่งขอมูลบนภาพต่อเนื่องเป็นไม่ต่อเนื่องเพื่อบันทึกค่าแสง
- Image pixel coordinate system จุดกำเนิด (0,0) อยู่ซ้ายบน
- Signed Distance Function ประมวลผลโดยใช้หน่วยความจำมากกว่า แต่เร็วกว่า Poisson Surface Reconstruction 
- Marching cubes แปลง occupancy grid ของ implicit function ไปเป็น surface
- Spatial filtering แบบ medium filter กำจัด impulse noise ได้
- Perspective Transform เปลี่ยนแปลงมุมกล้องของกล้องที่ถ่ายภาพ
- Caffe : Deep Learning Framework สำหรับงานด้าน Image Processing
- Generative Adversarial Networks (GAN)
	- ใช้ในงาน Super Resolution Reconstruction
	- ใช้ pre-trained model ได้
	- นำ generator อย่างเดียวไปใช้งานได้
	- ต้องมี network อย่างน้อย 2 มี loss function มากกว่า 1
- Principle Component Analysis (PCA) 
	- นำไปใช้จำใบหน้าภายใต้สภาวะแสงหลากหลาย เลือก eigenvectors ที่สอดคล้องกับ eigenvalue ต่ำ จะได้ประสิทธิภาพสูง เนื่องจากค่าที่ได้จาก eigenvalue สูง จะมี variance ของแสงมาก
	- แก้ computational complexity ด้วยการสุ่มรูปภาพ
- f(u,v)=x ไม่ใช่ Implicit surface function
## Point Cloud
- ประกอบด้วย (x, y, z) แสดงรูปทรงหรือพื้นผิววัตถุ
- สามารถแปลงเป็น Mesh, Voxel Grid, Depth ได้โดยตรง
- Vanilla ICP (Iterative Closest Point) ประกอบด้วย Data Association และ Alignment
## Virtual Influencer
- เหมาะกับงาน Marketing
- ข้อดีคือประหยัด ควบคุมพฤติกรรมได้ ไม่ต้องคอยกำกับดูแล
- เช่น Ai Ailynn, Bangkok Naughty Boo, Wunni
## Blockchain
- Bitcoin ตรวจสอบประวัติธุรกรรมของแต่ละคนได้
- Hash แบบ SHA256
	- ดีกว่า Encryption/Decryption ตรวจจับการเปลี่ยนแปลงข้อมูลได้ดีกว่า เหมาะกับ integrity check
	- สามารถถูกโจมตีแบบ brute-force ได้
- Crypto Wallet หาก Metamask ปิด เรายังเข้า wallet ของเราได้ โดยใช้ seed phrase บนกระเป๋าอื่น
- Hot Wallet ต่อเนตเสมอ Cold Wallet ไม่เชื่อมต่อ
- Hardware Wallet ปลอดภัยกว่า Software Wallet
## Docker
- ใช้ทรัพยากรน้อยกว่า VM
- สถาปัตยกรรมประกอบด้วย Images, Containers, Registry
- ดู log การทำงานได้ด้วยคำสั่ง `docker log container_id`
## Microservice
- ไม่ควรใช้ synchronous ที่ทำให้ services ที่เรียกตจ่อๆกันรอคำตอบ ใช้งานได้ไม่เต็มที่
- API สร้าง user ใหม่สำเร็จ ควร return http response code เป็น 201
## Internet of Things and Robotics
Embedded System : ระบบสมองกลฝังตัว เป็นหมวดนึงของ Computer System
- Software: Arduino, C++, Rust
- Hardware: Arduino Board, Raspberry Pi, ESP8266
- มีวงจรที่ทำงานบน Logic 1 (High), 0 (Low)
- Closed Loop Control System need Feedback
Internet of Things (IoT) 
- Network of Physical System
- Embedded System + Connectivity (ต่อ Internet)
- Wireless Communication: Wifi, Bluetooth, Cellular
AIoT : Hardware, Software, Connectivity, Data Analytics and Intelligence
Robotics
- ทำงานในรูปแบบ
	- Sense
		- หุ่นยนต์เคลื่อนที่ ตรวจสอบระยะห่างด้วย Laser Range Scanner 
	- Think
	- Act
- Direct Kinematics : Forward from Angle of Joint to End-effector
- Backward Kinematics : End-effector to Angle of Joint
- Fixed Robot แบบ SCARA เหมาะกับพื้นที่จำกัดแนวตั้ง ความเร็วสูง payload หลากหลาย
- Actuator อุปกรณ์ต้นกำลัง แปลงพลังงาน เป็น การเคลื่อนไหวทางกล
	- BLDC Motor
	- AC Servo Motor
	- Pneumatic Cylinder
- Faults ประเภท Unbalance, Parallel Misalignment และ Angular Misalignment ทำให้เกิดการสั่นสะเทือนในแนวแกน Radial, Radial, Axial ตามลำดับ
- Articulate Robot คือแขนกลที่นิยมใช้ประกอบชิ้นส่วนยานยนต์
- Degree of Freedom บอกจำนวนการเคลื่อนที่อิสระที่ทำได้
	- 7-axis robot ยืดหยุ่นในการทำงานมากกว่า 6-axis robot
- Algorithm ของ Mobile Robot หุ่นยนต์แบบเคลื่อนที่
	- Obstacle avoidance
	- Localization
	- Mapping
	- Path Planning
		- Dijkstra Algorithm (Shortest Path)
- Breakdown Maintenance (BM) เน้นซ่อมเมื่อใช้งานไม่ได้ (fix when it fails) ไม่ต้องสำรองอะไหล่
NODE-RED
- support user authentication and event-driven, no need to connect every node
- function always `return msg;` to send data to next node
- 3 Context Scope level: global, flow, node
- need `node.js` to run
- use `mosquitto_pub` and `mosquitto_sub` to transmit
- ระดับการแสดงผล
	- tab หน้าจอหลัก จะไม่แสดงผล ถ้าไม่มี group เลย
	- group จัดกลุ่ม มีได้หลาย group ใน tag
	- widget แสดง ui ย่อย
Baud Rate คืออัตราเร็วในการรับส่งข้อมูลที่ต้องกำหนดค่า
## Assessment Review
[Acha](https://drive.google.com/drive/folders/10Xl5-qGjKP5OX6_4R7vgIb7M1B40xeXF)
[OAP - Three](https://docs.google.com/document/d/1oMQPiE8RH0IaSIHelnUnFmuiRRrGsfWHw22bk1AkrVM/edit?tab=t.0)