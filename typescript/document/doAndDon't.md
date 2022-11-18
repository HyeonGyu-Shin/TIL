<br>

## 🔎  TS 공식 문서 - Do’s and Don’t

<br>

### ✏️  일반 타입(General Types)

<br>

👉  Number, String, Boolean, Symbol, Object와 같은 Built-in 객체를 사용하면 안 된다!

대신 number, string, boolean, symbol, null or undefined, object와 같은 TS에서 제공해주는 빌트인 객체를 사용해야 한다.

<br>

```tsx
function reverse(s: String): String; // X
function reverse(s: string): string; // O
```

<br>

### ✏️  제너릭

<br>

👉  타입 매개변수를 사용하지 않는 제너릭 타입을 사용하면 안 된다.

<br>

### ✏️  콜백의 반환 타입(Return Types of Callbacks)

<br>

👉  사용하지 않는 콜백의 return 타입에 any를 사용하면 안 된다.

<br>

```tsx
// 잘못됨!
function fn(x: () => any) {
  x();
}
```

<br>

👉  사용하지 않는 콜백의 return 타입은 void를 사용해야 한다.

왜냐? void를 사용하면 실수로 x의 return 값을 사용하는 걸 방지할 수 있기 때문이다.

<br>

```tsx
function fn(x: () => void){
	x();
}

---

function fn(x: () => void){
	let k = x();
	k.doSometing(); // error!
}
```

<br>

### ✏️  콜백에서 선택적 매개변수(Optional Parameters in Callbacks)

<br>

👉  정말 의도한 것이 아니라면 콜백에 optional을 사용하면 안 된다!

<br>

```tsx
// 잘못됨
interface Fetcher {
  getObject(done: (data: any, elapsedTime?: number) => void): void;
}
```

<br>

위의 함수는 구체적인 의미를 띈다. done 콜백은 1개 혹은 2개의 인자로 호출될 수 있다. 아마 elapsedTime 매개변수가 콜백과의 상관이 없다는 걸 나타내려는 의도였을 것이다.

하지만 이러한 의도를 만들기 위해 매개변수를 optional로 만들 필요는 없다. 왜냐면 정의한 인수보다 더 적은 인수를 콜백에 제공하는 건 항상 허용되기 때문이다.

<br>

```tsx
interface Fetcher {
  getObject(done: (data: any, elapsedTime: number) => void): void;
}
```

<br>

### ✏️  오버로드와 콜백(Overloads and Callbacks)

<br>

👉  콜백의 인수만 다른 오버로드를 분리해서 작성하면 안 된다!

<br>

```tsx
// 잘못됨
declare function beforeAll(action: () => void, timeout?: number): void;
declare function beforeAll(
  action: (done: DoneFn) => void,
  timeout?: number
): void;
```

<br>

최대 인수를 사용해 하나의 오버로드를 작성해야 한다.

<br>

```tsx
declare function beforeAll(
  action: (done: DoneFn) => void,
  timeout?: number
): void;
```

<br>

콜백이 매개변수를 무시하는 건 항상 허용됨으로, 굳이 짧은 오버로드를 필요로 하지 않는다. 또한, 더 짧은 콜백을 먼저 작성하면 넘어오는 함수가 첫 번째 오버로드와 일치하기 때문에 잘못된 타입의 함수를 허용할 수 있다.

<br>

### ✏️  함수 오버로드 순서(Function Overloads Ordering)

<br>

👉  더 넓은 범위의 오버로드를 더 좁은 범위의 오버로드 이전에 두면 안된다. 즉, 좁은 범위 → 넓은 범위의 순서로 작성해야 한다.

<br>

```tsx
/* 잘못됨 */
declare function fn(x: any): any;
declare function fn(x: HTMLElement): number;
declare function fn(x: HTMLDivElement): string;

var myElem: HTMLDivElement;
var x = fn(myElem); // x: any, wat?
```

<br>

```tsx
/* 좋음 */
declare function fn(x: HTMLDivElement): string;
declare function fn(x: HTMLElement): number;
declare function fn(x: any): any;

var myElem: HTMLDivElement;
var x = fn(myElem); // x: string, :)
```

<br>

왜냐하면 TS는 함수 호출을 처리할 때 첫 번째로 일치하는 오버로드를 선택한다. 넓은 범위의 오버로드가 앞에 있다면 위의 오버로드들은 가려져서 호출되지 않는다.

<br>

### ✏️  선택적 매개변수 사용(Use Optional Parameters)

<br>

👉  뒤따라오는 매개변수만 다른 오버로드를 작성하면 안 된다.

<br>

```tsx
/* 잘못됨 */
interface Example {
  diff(one: string): number;
  diff(one: string, two: string): number;
  diff(one: string, two: string, three: boolean): number;
}
```

<br>

가능한 선택적 매개변수를 사용해야 한다.

<br>

```tsx
/* 좋음 */
interface Example {
  diff(one: string, two?: string, three?: boolean): number;
}
```

<br>

### ✏️  유니언 타입 사용 (Use Union Types)

<br>

👉  한 인수 위치에서 타입만 다른 오버로드를 사용하면 안 된다.

<br>

```tsx
/* 잘못됨 */
interface Moment {
  utcOffset(): number;
  utcOffset(b: number): Moment;
  utcOffset(b: string): Moment;
}
```

<br>

가능한 유니언 타입을 사용해야 한다.

<br>

```tsx
/* 좋음 */
interface Moment {
  utcOffset(): number;
  utcOffset(b: number | string): Moment;
}
```

<br>

## 📌  정리

<br>

👉  오버로드를 할 바엔 optional을 사용하고, 되도록이면 optional을 사용하지 말자!
