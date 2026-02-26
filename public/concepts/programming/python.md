---
title: "Python"
tags: [concept, programming]
created: 2026-02-26
---

# Python

## Documentation
```python
help(list) # ใช้ help เพื่อดูคำอธิบาย
```
## Data Types & Method
```python
# Boolean
isWorking = True
# String
name = 'Fai'
name.lower()
name.find('F')
name.split() # แบ่งคำตามคำที่กำหนด default คือช่องว่าง
name.lstrip()
name.rstrip()
# Integer
age = 35
# List
food = ['cake','noodle']
food.push('fried rice')
food.pop()
# Dictionary
favorite = {
	'Artist': 'Jessica',
	'Actress': 'Krystal',
}
```
## Input & Output

```python
data = input()
print(data)

```
## Control Flow
```python
if something == False:
	print(nothing)

for menu in food:
	print(menu)
```
## Function
```python
def greet(name):
	print("Hello" + name)
```
