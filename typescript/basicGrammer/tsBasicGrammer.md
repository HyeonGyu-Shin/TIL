<br>

# TS 기본 문법

<br>

👉  기본적으로 variable, parameter, return 값에 type이 붙었다고 생각하면 된다.

```tsx
const a: number = 5;
function add(x: number, y: number): number {
  return x + y;
}
const add: (x: number, y: number) => number = (x, y) => x + y;
const obj: { lat: number; lon: number } = { lat: 37.5, lon: 127.5 };
```

👉  js의 원시값 외에 특수한 타입인 {}를 사용할 수 있다.

```tsx
const a: {} = 5;
const b: {} = 'hello';
```

다만 권장하지는 않을 것 같다는 생각이 든다.

👉  ts는 알아서 타입을 추론해준다. 추론한 결과가 맞다면 그냥 사용하면 된다. 만약 ts가 추론하지 못 할 경우에만 직접 추론을 한다.

```tsx
const a = 5; // number
const b = '3'; // string
const c = a + b; // string
function add(x: number, y: number) {
  return x + y;
}
```

👉  : 뒷부분, as 뒷부분, <>부분, interface, type, function 일부를 제외하면 JS와 동일해진다.

위에서 언급한 부분을 제외하고 생각하는 연습을 하는 것이 좋다.

```tsx
const obj: { lat: number; lon: number } = { lat: 37.5, lon: 127.5 };
const obj = { lat: 37.5, long: 127.5 };

const a = document.querySelector('#root') as HTMLDivElement;
const a = document.querySelector('#root');

function add<T>(x: T, y: T): T {
  return x + y;
}
function add(x, y) {
  return x + y;
}

interface A {}
type A = {};
```

👉  any는 최대한 쓰지 말자! 이를 사용하는 건 그냥 쌩 JS를 사용하는 것과 같다.

또한 never, unknown 타입도 주의하자!

```tsx
try {
  const array = []; // noImplicitAny가 false일 때, 빈 배열은 never를 나타낸다...?
  array[0];
} catch (error) {
  error;
}
```

👉  최대한 ! 대신 if를 사용하자

!는 내가 정말 이 값이 존재한다는 걸 보장할 때 사용한다. 하지만 현실에서 이를 보장한다는 건 거의 불가능하기 때문에 if를 사용하자!

```tsx
const head = document.querySelector('#head')!;
console.log(head);

const head = document.querySelector('#head');
if (head) {
  console.log(head);
}
```

👉  string과 String은 다르다.

String은 빌트인 객체이다. 따라서 둘은 다르다.

```tsx
const a: string = 'hello'; // O
const b: String = 'hell'; // X
```

👉  템플릿 리터럴 타입이 존재한다.

```tsx
type World = 'world' | 'hell';

// type Greetting = 'hello world'
type Gretting = `hello ${World}`;
```

👉  배열, 튜플 문법

```tsx
let arr: string[] = [];
let arr2: Array<string> = [];
function rest(...args: string[]) {}

const tuple: [string, number] = ['1', 1];
tuple[2] = 'hello'; // 에러!
tuple.push('hello'); // 이건 tsc에서 잡아주지 못한다 ㅠㅠ 이런 바보같은!
```

👉  enum, keyof, typeof

enum은 자바를 했을 때 잠깐 사용해본 것 같다. enum을 사용하기 싫다면 keyof, typeof를 사용하면 된다.

```tsx
const enum EDirection {
  Up,
  Down,
  Left,
  Right,
}

const ODirection = {
  Up: 0,
  Down: 1,
  Left: 2,
  Right: 3,
} as const;

EDirection.Up;

(enum member) EDirection.Up = 0

ODirection.Up;

(property) Up: 0

// Using the enum as a parameter
function walk(dir: EDirection) {}

// keyof는 객체 타입에서 key값을 가져온다.
// typeof는 객체 타입을 나타낸다.
type Direction = typeof ODirection[keyof typeof ODirection];
function run(dir: Direction) {}

walk(EDirection.Left);
run(ODirection.Right);
```

👉  union

union은 “또는”이라 생각하면 된다. 여러 개 중 어떤 한 값이 들어올 수 있다는 뜻이다.

```tsx
function add(x: string | number, y: string | number): string | number {
  return x + y;
}
add(1, 2);
add('1', '2');
add(1, '2');
```

👉  intersection과 객체의 잉여 속성 검사

더 자세한 내용은 여기를 눌러 살펴보자.

```tsx
type A = {
  a: string;
};

type B = {
  b: string;
};

const aa: A | B = { a: 'hello', b: 'world' };
const bb: A & B = { a: 'hello', b: 'world' };
```

👉  interface끼리는 서로 합쳐진다.

```tsx
interface A {
  a: string;
}
interface B {
  b: string;
}
const obj1: A = { a: 'hello', b: 'world' };

// type은 에러가 발생한다!
type B = { a: string };
type B = { b: string };
const obj2: B = { a: 'hello', b: 'world' };
```

👉  void 타입은 return 값을 사용하지 않겠다는 뜻이다.

다만 메서드나 매개변수에서는 return 값을 사용 가능하다고 하니 조심해야 한다…?

더 자세한 내용은 [여기](https://www.notion.so/TS-void-0ebcce8446ba4ed7afe9ac433dc6a01f)를 통해 살펴보자!

```tsx
declare function forEach<T>(arr: T[], callback: (el: T) => undefined): void;
// declare function forEach<T>(arr: T[], callback: (el: T) => void): void;
let target: number[] = [];
forEach([1, 2, 3], (el) => target.push(el));

interface A {
  talk: () => void;
}
const a: A = {
  talk() {
    return 3;
  },
};
```

👉  타입만 선언하고 싶을 대 declare를 선언한다.

다만 구현은 다른 파일에 있어야 한다.

예를 들어, 브라우저에서 여러 script를 통해 모듈을 가져오는데,다른 모듈에 구현체가 있다면 이를 이용하는게 제일 베스트이다.

허나 이를 tsc가 알 방법이 없다. 하지만 declare를 통해 다른 모듈에 구현체가 있다는 것을 tsc에 알릴 수 있다.

```tsx
declare const a: string;
declare function a(x: number): number;
declare class A {}
```

👉  any와 unknown의 차이점!

- any는 tsc가 타입검사를 포기한다.
- unknown은 사용자가 직접 타입을 넣어줘야 한다.

👉  타입 가드

TS는 타입 구분을 잘 해준다!
자세한 내용은 [여기](https://www.notion.so/e605d9bf602d479cb9b23e64893c9a0f)를 참고하자!

👉  {}와 Object 타입

{}과 Object는 null과 undefined를 제외한 모든 타입을 뜻한다.

object가 객체를 뜻하고, Object는 모든 타입을 뜻한다.

👉  readonly

```tsx
interface A {
  readonly a: string;
  b: string;
}

const aaa: A = { a: 'hello', b: 'world' };

// 에러 발생!
// readonly 때문에!
aaa.a = '123';
```

👉  index signiture

```tsx
// 모든 객체의 속성의 key 값과 value 값을 정의한 값으로 고정한다.
type A = { [key: string]: string };

const aaa: A = { a: 'hello', b: 'world' };

// 에러 발생!
// key와 value가 string이 와야하기 때문에!
const bbb: A = { a: 123 };
```

👉  mapped types

```tsx
// B에 정의한 값들을 key 값으로 무조건 포함해야 한다!
type B = 'Human' | 'Mammal' | 'Animal';
type A = { [key in B]: number };

const aaa: A = { Human: 1, Mammal: 2, Animal: 3}

---

// value의 값도 제한할 수 있다.
type B = 'Human' | 'Mammal' | 'Animal';
type A = { [key in B]: B };

const aaa: A = {
	Human: 'Animal',
	Mammal: 'Human',
	Animal: 'Mammal',
}
```

👉  TS에서의 class

private, protected, abstract class 등의 개념이 추가되었다.

자세한 사항은 [여기](https://www.notion.so/TS-class-29b371a5b654432ea2ddc12c383ee0cc)에서 살펴보자.

👉  optional은 있어도 되고, 없어도 된다는 의미를 나타낸다.

```tsx
function abc(a: number, b?: number, c?:number){}

abc(1)
abc(1, 2)
abc(1, 2, 3)

// 에러 발생!!
abc(1, 2, 3, 4)

---

function abc(...args: number[]){}

abc(1)
abc(1, 2)
abc(1, 2, 3)

// 성공적으로 여러 개의 인수를 받아올 수 있다.
abc(1, 2, 3, 4)
```

결국 optional은 매개변수를 정확하게 컨트롤 하는 데에 이용한다.

👉  [제너릭](https://www.notion.so/Generic-9a61f673c12b42d489c30e141f2fa317)

👉  기본값 타이핑

`매개변수: 타입 = 기본값` 의 형태이다.

제너릭에 기본값은 넣어주는게 좋다…?
<T = unknown> 값이 사용될 때 덮어쓰기를 해서 상관없다!
