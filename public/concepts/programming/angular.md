---
title: "Angular"
tags: [concept, programming, frontend]
created: 2026-02-26
---

# Angular

## Resources
- [Angular](https://angular.dev/)
- [Tutorial](https://angular.dev/tutorials/learn-angular/)
- [Develop with AI](https://angular.dev/ai/develop-with-ai )

Front End : Angular <-> Back End : Nest
`ng new project-name`

## Components
`ng generate component`
```ts
@Input()
@Output()
```

Binding
```ts
[inputBinding]="statement"
(outputBinding)="statement"
[(twoWayBinding)]="statement"
```

Control Flow in Components

If Else
สำหรับเวอร์ชั่น 16 หรือเก่ากว่า ใช้ในรูปแบบ Structural Directive `*ngIf`
สำหรับเวอร์ชั่นล่าสุด ใช้ `@If`
```ts
template: `
  @if (isServerRunning) { ... }
  @else { ... }
`;
```

ng-template เอาไว้สร้างเป็น template จะไม่ถูกสร้างจนกว่าจะถูกนำไปเรียกใช้
```ts
<ng-template #elseTemplate>
</ng-template>
```

For
`*ngFor`

EventEmitter

Angular Proxy config
proxy.conf.json

get กับ set 
- จะคำนวณใหม่ทุกครั้ง หรือใช้กับการรับ input 
- ใช้กับการ ViewChild
```ts
get ชื่อตัวแปร() {
	return ค่า
}
set ชื่อตัวแปร(ค่า)
```
`ng generate service service-name`
Service
- ช่วยลดความซับซ้อนของโค้ดของเรา
- ใช้งานร่วมกันในหลายๆ components 
- ไม่ต้องผูกกับ Module 
- ไม่ต้อง declaration

ng generate directive directive-name
เพิ่มความสามารถของ element โดยมักตั้งชื่อตามความสามารถที่เพิ่มขึ้น

## Form
import มาใช้งานจาก `@angular/forms`
อ่านเพิ่มเติม `NG_VALUE_ACCESSOR`
### Forms Module
เหมาะกับงานที่ไม่ซับซ้อน
```ts
<input [(ngModel)]="variableName" />
```
เอาไว้แสดงผลค่าของตัวแปร
### Reactive Forms Module
รองรับงานที่ซับซ้อนมากกว่า
ใช้ Reactive Programming ปล่อยข้อมูลแบบ Stream (RxJs/ngRx)
```ts
exampleForm = new formControl(ค่าเริ่มต้น, [Validators.required]);
this.exampleForm.value

```
```ts
<input [formControl]="exampleForm" />
exampleForm.invalid
exampleForm.touched
{{ exampleForm.errors | json }}

```
#### Form Validation แบบต่างๆ
- ทุกครั้งที่มีค่าเข้ามาผ่าน Form Control ไม่ว่าจะผ่าน ts หรือ html ก็จะมาตรวจสอบผ่าน Validators ที่กำหนดไว้ แล้วไปอัพเดทค่าไปยัง object errors และค่า invalid
`Validators.required` บังคับกรอก
`Validators.email` เช็ครูปแบบ email
`Validators.minLength(n)` จำนวนตัวอักษรขั้นต่ำ
#### Custom Validator
`ValidatorFn`
สร้าง Function ที่รับค่าเป็น AbstractControl แล้วส่งค่าเป็น ValidationErrors หรือ null
```ts
idCardValidator: ValidatorFn = (control:AbstractControl) => {
	if (idCard) {
		if (idCard.length != 13) return { idCard: true }
		// logic เช็คอื่นๆ
		let index = 13;
		let sum = 0;
		for (const num of idCard.substring(0, 12)) {
			if (isNaN(Number(num)))
				return { idCard: true };
			sum += Number(num) * index--;
		}
		const mod = sum % 11;
		const last = `${11 - mod}`.slidce(-1);
		if (idCard.charAt(12) !== last) return { idCard: true }
	}
	// 1234567890121
	return null;
}
```

### @Pipe()
ใช้เพื่อแปรข้อมูลให้อ่านง่ายขึ้น มี built-in pipe ที่พร้อมใช้ได้เลย ดูเพิ่มเติม Angular Pipe
สร้างโดยใช้
```
ng generate module shared/pipe/pipe-name
ng generate pipe pipe-name --export=true
```
การสร้างรูปแบบการเปลี่ยนแปลง
```ts
transform(vale:)
```

Class Style Binding
```html
[class.ชื่อคลาส]="active"
```
```css
transform: translateX(100)
transition: transform 0.4s ease-out;
```

ViewChild
สามารถอ้างอิงแบบ HTML Element หรือ TS Class Component ก็ได้
```ts
@ViewChild('childName', {read:ElementRef}) imageRef?: ElementRef<HTMLImageElement> {}

imageRef.nativeElement
```
```html
<img #childName />
```
# RxJS

# Angular CDK 
Component Development Kit
- DragDropModule `@angular/cdk/drag-drop`
	- ใช้กับ ng-container แล้วแปะ directive ได้ เพราะ ng-container 
## UI Components

Prime NG
