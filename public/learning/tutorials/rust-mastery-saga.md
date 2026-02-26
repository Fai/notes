---
title: "Rust Mastery Saga"
tags: [learning, tutorial, rust]
created: 2026-02-26
---

# Rust Mastery Saga

## Module 1
## ทำไมต้องเขียน Rust
- Rustacean คนที่เขียน Rust
- Ferris เป็น Mascot
- ปัญหาของ Stack กับ Heap ในภาษา C ที่คนต้องจัดการ  memory เอง
- Rust จัดการให้ที่ compile time ด้วยระบบ ownership ทำให้ใช้งาน heap ตัวเดียวกันไม่ได้ ทำให้ปลอดภัย ไม่ต้องใช้ Garbage Collector
- WebAssembly (WASM) เป็น runtime ที่ Build แปลง Rust Code เป็น binary instructions ทำงานคู่กับ JS บน Browser
## Requirement
1. [Rust](https://doc.rust-lang.org/book/) เป็นภาษาโปรแกรม
2. [PostgreSQL](https://www.postgresql.org/download/) ติดตั้งเพื่อที่จะใช้กับ Diesel (ORM) ตอนทำ Final Project
3. [Podman](https://podman.io/docs/installation) หรือ Docker เป็น Container Application ใช้จำลอง Database และ Build Project เพื่อทำการ Deploy
4. [Postman](https://www.postman.com/downloads/) ใช้ทดสอบ API
5. [VS Code](https://code.visualstudio.com/docs/setup/setup-overview) พร้อมติดตั้ง Rust Analyzer Extension เป็น IDE สำหรับใช้เขียน Code ได้หลายภาษา รวมถึง Rust
6. [DBeaver](https://dbeaver.io/download/) ใช้ส่อง Database
7. [Visual Studio](https://visualstudio.microsoft.com/downloads/) ในกรณีที่ไม่สามารถติดตั้ง หรือ Build Rust ได้ ให้ติดตั้งและเลือกลง Build Tools ของ C++ ด้วย เอาไว้เผื่อในกรณีที่เจอปัญหา Build Rust ไม่ได้
8. [Git](https://git-scm.com/downloads) ใช้สำหรับทำ Version Control และ Push Code ขึ้น Github
## Support

Pattern การตั้งคำถาม
1. ติดปัญหาอะไร
2. Code เขียนไว้อย่างไร หรือถ้าไม่มี Code ก็ไม่เป็นไร
3. ลองพยายามแก้ไขอะไรไปแล้วบ้าง
## Module 2
## Website
- [[HTML]]
- [[CSS]]
- [[JavaScript]]
## Frontend
Tech Stack : HTML, CSS, JavaScript, Framework
## Backend
Tech Stack : Rust, Kotlin, Go, Database, Infrastructure
## [[OSI Model]]
ประกอบด้วย 7 Layers
- Application 
- Presentation
- Session
- Transport เป็น Protocol ที่ใช้งาน เช่น TCP, UDP
- Network ใช้ IP Address เพื่อวางแผน Route ผ่าน Router
- Data Link ใช้ MAC Address เพื่อตรวจสอบ Identity ว่าต้องไปที่ Device ไหน ผ่าน Switch
- Physical อุปกรณ์ส่งสัญญาณ
TCP/IP Model
การใช้งานจริง รวม Presentation และ Session ไปกับ Application
- Application 
- Transport 
- Network 
- Data Link 
- Physical 
[[UDP (User Datagram Protocol)]]
- Client ส่ง Request ไป Server Response กลับมา
- Connectionless
- Fast, Risky
[[TCP (Transmission Control Protocol)]]
- Client ส่ง Syn ไปก่อนเพื่อดูความพร้อมของ Server แล้วให้ Server ส่ง Syn-Ack มาก่อนที่จะ Client จะเริ่มเชื่อมต่อด้วย Ack
- ปลอดภัยกว่า

[[HTTP (Hyper Text Transfer Protocol)]]
- ก่อนเริ่มใช้งาน จะเชื่อมต่อกันด้วย TCP ก่อนเสมอ
- จากนั้น Client จะส่ง HTTP Request ไป แล้ว Server จะส่งข้อมูลกลับมาพร้อม HTTP Status Code
- ใช้กับการส่งข้อมูลบน Web และ API

[[HTTPS (Hyper Text Transfer Protocol Secure)]]
- HTTP + SSL
- ใช้ Encryption เพื่อเข้ารหัส Data ที่ส่งหากันระหว่าง Client และ Server
[[API (Application Programming Interface)]]
- [[RESTful API]]
- [[JWT (JSON Web Token)]] ใช้ยืนยันตัวตน

## Module 3

[[Rust]] 101
- สร้างโปรเจคใหม่ด้วย `cargo new ชื่อโปรเจค`
- เริ่มต้นด้วย `main.rs` และ `Cargo.toml`
- ใช้ `cargo run` สำหรับการรันโปรเจค
- เราประกาศตัวแปรด้วย `let ชื่อตัวแปร:ชนิด = ค่า`
- ปกติตัวแปรเป็น Immutable ไม่สามารถเปลี่ยนแปลงค่าได้
- ถ้าต้องการให้เปลี่ยนค่าได้ ให้ประกาศเป็น `mut`

ประเภทของตัวแปร แบ่งออกเป็น
- `i32` เป็นจำนวนนับ integer ขนาด 32 bits 
- `f64` เป็นจำนวนเต็ม floating point ขนาด 64 bit
- `string` เก็บใน heap
- `&str` 
- เราสามารถ cast type ได้โดยการใช้ `as`

Control Flow ทำงานตาม condition
- `if-else` ถ้าเป็นไปตามเงื่อนไข ให้ทำงาน
```rust
if เงื่อนไขแรก {
	ทำงานแบบแรก
} else if เงื่อนไขที่สอง {
	ทำงานแบบที่สอง
} else {
	ทำงานแบบที่สาม
}
```
- `match` มีเงื่อนไขหลายชนิด เหมือน switch case
```
match ชื่อตัวแปรที่เช็ค {
	ค่าที่หนึ่ง => ทำงานแบบแรก,
	ค่าที่สอง => ทำงานแบบที่สอง,
	ค่าที่สาม => ทำงานแบบที่สาม,
	_ => ทำงานแบบปกติ,
}
```
- `loops` ทำงานไปเรื่อยๆ ไม่สิ้นสุด
```
loop {
	ทำงาน
	if เงื่อนไขที่จบ {
		break;
	}
}
```
- break ออกจาก loop
- continue เอาไว้ข้ามไป loop ต่อไป
- while ทำงานตราบใดที่อยู่ในเงื่อนไข
```

```
- for ทำตามจำนวนที่ตั้งไว้
```
for item in items.iter() {

}
```

ภาษา Low-Level แบบเดิม
- Stack เป็น High Address จัดการอัตโนมัติ
- Heap เป็น Low Address จัดการเอง
ภาษา High Level
- ใช้ Garbage Collector จัดการ Memory
- กระทบ Performance
ภาษา Rust
- มี Memory Management System
- ใช้ Ownership & Borrowing

Ownership
- ทุกค่าจะมีเจ้าของเป็นตัวแปรเสมอ
- แต่ละค่ามีเจ้าของได้แค่ตัวแปรเดียว
- ความเป็นเจ้าของจะจบลงเมื่อตัวแปรอยู่นอก scope และข้อมูลจะถูกเคลียร์โดยอัตโนมัติ
Borrowing
- เราสามารถยืมค่าได้โดยการอ้างผ่าน `&`
- เจ้าของให้ยืมได้ทั้งแบบ immutable และ mutable เพื่อบอกว่าคนที่ยืม สามารถแก้ไขค่าได้ไหม โดยถ้ายืมแบบ mut จะยืมได้ทีละคนเท่านั้น
- Lifetime ใช้บอกว่าจะ Reference นานแค่ไหน โดยใส่ `'ตัวอักษร` เพื่อให้ทุกตัวที่อ้างถึงมี Lifetime เท่ากัน ตามตัวที่มี Lifetime มากที่สุด

Structs
- เป็นเหมือน blueprint
- ใช้จัดกลุ่ม data หลายๆตัวแปร ไว้ด้วยกัน เพื่อสร้าง custom types
- ชื่อจะเป็น Capital ขึ้นด้วยตัวใหญ่
```rust
struct Crabby {
	name: String,
	health: u8,
}

impl Crabby {
	fn take_damange(&mut self, damange: u8) {
		self.health = self.health.saturating_sub(damage);
	}
	fn healing(&mut self, heal: u8) {
		if self.health + heal >= 100 {
			self.heath = 100;
			return;
		}
		self.health += heal;
	}
}

fn main() {
	let mut crabby: Crabby = Crabby {
		name: "Crabby".to_string(),
		health: 100
	}
}
```
- สามารถประกาศเป็น Tuple Struct ได้

Enums (Enumeration)
- แสดง states ได้หลากหลาย
```rust
enum CrabbyState {
	Resting,
	Fighting,
	Collecting(u32),
	Defending,
}

fn main() {
	let crabbyState = CrabbyState::Fighting;
}
```

Traits
- กลุ่มของความสามารถ หรือพฤติกรรม ที่ทำให้ structs หรือ enums มาใช้ได้
- ช่วยลด duplicate code ในการทำงานที่คล้ายคลึงกัน
```
trait TraitName {

}

impl ชื่อ_traits for ชื่อ_struct {

}
```

Generics
- ทำให้ struct หรือ function ทำงานกับ type ได้หลายๆ type `<T>`
- ช่วยให้ type ใช้งานได้ยืดหยุ่น

String 
- String ปกติ
- &str (Borrowed String or String Slice)

Vector
- Dynamic array, owned and heap-allocated
- ย่อ ขยาย ปรับขนาดได้
- สามารถให้ยืมได้ด้วย
```rust
fn main() {
	let mut treasures: Vec<String> = Vec::new();
	let mut my_treasure: vec!["Gold", "Silver"]
	
	treasures.push(String::from("Gem"));
	my_treasure.pop();
	my_treasure.remove(0);
}
```
- `.len()` Length ปัจจุบันยาวเท่าไหร่ 
- `.capacity()` Capacity ปัจจุบันจุได้เท่าไหร่ 
	- บางทีถ้า pop ออกไป แต่ยังจองพื้นที่ไว้ len จะลด แต่ capacity เท่าเดิม
	- `.remove()` จะได้ capacity ลดลง
- `.iter()` เป็นการ borrowing ค่าทีละ index
- `.into_iter()` เป็นการ moving ownership ค่าทีละ index
- `.map()`
- `.collect()` แปลงค่าจาก map ให้เป็น vector ตามเดิม

Closures
- เป็นเหมือน inline function
```rust
let add = |x, y| x + y;
let result = add(3, 4);
```
- ใช้ใน map คู่กับ Iterators ก็ได้
```rust
fn main() {
	let treasures: Vec<i32> = vec![100, 200, 300, 400];
	let doubled_treasures: Vec<i32> = treasures.iter().map(|x| x * 2).collect();
}
```

HashMap
- key-value pair 
- เข้าถึงได้ไว แต่ใช้ memory เยอะ
- เวลาใช้ ต้อง import ก่อนด้วย `use`
```rust
use std::collections::HashMap;

fn main() {
	let mut treasures: HashMap<&str, i32> = HashMap::new();
	
	treasures.insert("Gold", 10);
	treasures.insert("Gem", 1000);
	
	if let Some(gem) = treasures.get("Gem") {
		println!("Gem: {}", gem);
	}
	let gem_count = treasures.get("Gem"),unwrap_or(&0);

	if let Some(gold) = treasures.get_mut("Gold") {
		*gold += 1000;
		println!("Gold: {}", gold);
	}
	treasures.remove("Gold");
}
```
- Some เป็น Option เผื่อหา key ไม่เจอ
- สามารถวนลูป `(key, value)` ได้
```
for (treasure, count) in treasures {

}
```

Error Handling

match some
```
let treasure = match check_result {
	Ok => message,
	Err(error) => panic!("Error");
}
```
if let
```
if let Some(treasure) = tres
```
Result Type 
- จะเป็น `Result<ok, err>`
```
fn open_chest(is_Locked: bool) -> Result<String, String>
```
unwrap_or หรือ unwrap_or_else ตั้งค่า default ไว้
```
unwrap();
unwrap_or()
unwrap_or_else(|| )
```
กรณีที่ ok กับ error เป็น type เดียวกัน
```
let chest_result = open_chest(false)?
```
