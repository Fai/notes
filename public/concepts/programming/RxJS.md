https://rxjs.dev
# Observable

Promise -> Callback
- `resolve()` ส่งผลลัพธ์กลับได้ครั้งเดียว และสถานะเสร็จสิ้น
- `reject()` ส่งกลับว่าไม่สำเร็จ

Observable -> Observe
- `observer.next()` ส่งผลลัพธ์กลับได้หลายครั้ง
- `observer.complete()`
- `observer.error()`
- `observable.subscribe()`
- `observable.unsubscribe()`

```ts
interface Observer {
	next:: (value:any) => void;
	error: (err:any) => void;
	complete: ()=> void;
}

class Subscription {
	teardownList: TearDown[] = [];
	constructor(teardown: TearDown) {
		if (teardown)
			this.teardownList.push(teardown);
	}
	add(subscription: Subscription) {
		this.teardownList.push(() => subscription.unsubscribe());
	}
	unsubscribe() {
		this.teardownList.forEach(teardown => teardown())
	}
}

class Observable {
	subscriber: (observer: Observer) => TearDown;
	constructor (subscriber: (observer: Observer) => Teardown) {
		this.subscriber = subscriber;
	}
	subscribe() {
		const subscription = new Subscripton(teardown);
	}
}

Observable.subscribe(() => {
	next: () => {},
	error: () => {},
	complete: () => {}
})	

function promiseToObserable(promise: Promise<T>) {
	return new Obserable(observer => {
		let isUnsubscribed = false;
		promise.then(data => {
			if (isUnsubscribed) {
			observer.next(data);
			observer.complete());
			}
		}).catch(err=>{
			observer.error(err);
		})
		return ()=> {
			isUnscribed = true;
		}
	})
}
```
## Observable กับ Operator Function
https://www.rxjs-fruits.com/subscribe

pipe()
- จัดรูปแบบข้อมูลที่ออกมาให้ตรงกับที่ต้องการ

เวลาเราใช้
```ts
observable.pipe(

).subscribe
```
distinct()
- เฉพาะตัวที่ unique
filter()
- ใช้เงื่อนไขที่เป็นจริงหรือเท็จในการกรองผลลัพธ์
map()
- ใช้ function บางอย่าง apply ไปที่ผลลัพธ์
concatMap()
- ทำ observable ทีละอัน
exhaustMap()
switchMap()
- ทำ observable ล่าสุด
take(n)
- เอาแค่ n ตัวจากผลลัพธ์ทั้งหมด
tap()
- 
share()
- **multicast** : ทุก observer ที่ subscribe observable ตัวเดียวกัน ก็จะได้ข้อมูลเดียวกัน ได้เวลาเดียวกัน
- มี subject ไป subscribe observable ก่อน แล้วที่เหลือค่อยมา subscribe
debounceTime ค้นหาหลังเราปล่อยมือแล้วหมดเวลาที่รอ
auditTime ค้นหาทุกๆเวลาที่กำหนด

## State Management
ใน Angular
- Built-in RxJS: multicast, Subject, Behavior
- External Library: NgRX, Akita, NGXS

State Management Lifecycle
- จาก component เมื่อมี actio (subject) n เปลี่ยนเป็นข้อมูลด้วย Reducer แล้วเก็บข้อมูล (state) ไว้ที่ store (behavior)
	- ใน action เกิด effects ที่มีการขอข้อมูลจาก service (request) แล้วได้ผลลัพธ์กลับมาเป็น (response) แล้วจะเกิด action จาก effect ส่งกลับไป
- จาก component ที่ต้องการดึงข้อมูลข้อมูล (state) ด้วย selector

## Example Use-Case

Drag & Drop
- `mousedown`, `touchstart`
- `mousemove`, `touchmove`
- `mouseup`, `touchend`

```ts
import { fromEvent } from 'rxjs';

function 
const elementList = document.querySelectorAll<HTMLElement>(`[data-draggable]`)

from(elementList.values()).pipe(mergeMap(element=>dragndrop(element))).subscribe();

function dragndrop(element: HTMLElement) {
	const mousedown$ = fromEvent<MouseEvent>(element, 'mousedown').pipe(
	tap(event=>event.preventDefault())
	);
	const touchstart$ = fromEvent<TouchEvent>(element, 'touchstart').pipe(
	tap(event=>event.preventDefault())
	);
	const down$ = merge(mousedown$, touchstart$);
	
	const mousemove$ = fromEvent<MouseEvent>(document, 'mousemove');
	const touchmove$ = fromEvent<MouseEvent>(document, 'touchmove');
	const move$ = merge(mousemove$, touchmove$);
	
	const mouseup$ = fromEvent<MouseEvent>(document, 'mouseup')
	const touchend$ = fromEvent<MouseEvent>(document, 'touchend');
	const up$ = merge(mouseup$, touchend$)
	
	let stateX = 0;
	let stateY = 0;

	const getClient = (event: MouseEvent | TouchEvent) => {
		let clientX: number;
		let clientY: number;
		if (event instanceof TouchEvent) {
			clientX = event.touche[0].clientX;
			clientY = event.touche[0].clientY;
		} else {
			clientX = event.clientX;
			clientY = event.clientY;
		}
	};
	
	return down$
		.pipe(
			switchMap((downEvent) => {
				let translate: {translateX:number, translateY:number}
				const clientDown = getClient(downEvent);
				return move$.pipe(
					tap(moveEvent => {
						const clientMove = getClient(moveEvent);
						const translateX = clientMove.clickX - clientDown.clickX + stateX;
						const translateY = clientMove.clickY - clientDown.clickY + stateY;
						element.style.transform = `translate(${translateX}px, ${translateY})`;
						translate = {translateX, translateY};
					}),
					finalize(()=> {
						stateX = translate.translateX;
						stateY = translate.translateY;
					}),
					takeUntil(up$)
				);
			})
		)
}
```
