<br>

# TS ๊ธฐ๋ณธ ๋ฌธ๋ฒ

<br>

๐ย  ๊ธฐ๋ณธ์ ์ผ๋ก variable, parameter, return ๊ฐ์ type์ด ๋ถ์๋ค๊ณ  ์๊ฐํ๋ฉด ๋๋ค.

```tsx
const a: number = 5;
function add(x: number, y: number): number {
  return x + y;
}
const add: (x: number, y: number) => number = (x, y) => x + y;
const obj: { lat: number; lon: number } = { lat: 37.5, lon: 127.5 };
```

๐ย  js์ ์์๊ฐ ์ธ์ ํน์ํ ํ์์ธ {}๋ฅผ ์ฌ์ฉํ  ์ ์๋ค.

```tsx
const a: {} = 5;
const b: {} = 'hello';
```

๋ค๋ง ๊ถ์ฅํ์ง๋ ์์ ๊ฒ ๊ฐ๋ค๋ ์๊ฐ์ด ๋ ๋ค.

๐ย  ts๋ ์์์ ํ์์ ์ถ๋ก ํด์ค๋ค. ์ถ๋ก ํ ๊ฒฐ๊ณผ๊ฐ ๋ง๋ค๋ฉด ๊ทธ๋ฅ ์ฌ์ฉํ๋ฉด ๋๋ค. ๋ง์ฝ ts๊ฐ ์ถ๋ก ํ์ง ๋ชป ํ  ๊ฒฝ์ฐ์๋ง ์ง์  ์ถ๋ก ์ ํ๋ค.

```tsx
const a = 5; // number
const b = '3'; // string
const c = a + b; // string
function add(x: number, y: number) {
  return x + y;
}
```

๐ย  : ๋ท๋ถ๋ถ, as ๋ท๋ถ๋ถ, <>๋ถ๋ถ, interface, type, function ์ผ๋ถ๋ฅผ ์ ์ธํ๋ฉด JS์ ๋์ผํด์ง๋ค.

์์์ ์ธ๊ธํ ๋ถ๋ถ์ ์ ์ธํ๊ณ  ์๊ฐํ๋ ์ฐ์ต์ ํ๋ ๊ฒ์ด ์ข๋ค.

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

๐ย  any๋ ์ต๋ํ ์ฐ์ง ๋ง์! ์ด๋ฅผ ์ฌ์ฉํ๋ ๊ฑด ๊ทธ๋ฅ ์ฉ JS๋ฅผ ์ฌ์ฉํ๋ ๊ฒ๊ณผ ๊ฐ๋ค.

๋ํ never, unknown ํ์๋ ์ฃผ์ํ์!

```tsx
try {
  const array = []; // noImplicitAny๊ฐ false์ผ ๋, ๋น ๋ฐฐ์ด์ never๋ฅผ ๋ํ๋ธ๋ค...?
  array[0];
} catch (error) {
  error;
}
```

๐ย  ์ต๋ํ ! ๋์  if๋ฅผ ์ฌ์ฉํ์

!๋ ๋ด๊ฐ ์ ๋ง ์ด ๊ฐ์ด ์กด์ฌํ๋ค๋ ๊ฑธ ๋ณด์ฅํ  ๋ ์ฌ์ฉํ๋ค. ํ์ง๋ง ํ์ค์์ ์ด๋ฅผ ๋ณด์ฅํ๋ค๋ ๊ฑด ๊ฑฐ์ ๋ถ๊ฐ๋ฅํ๊ธฐ ๋๋ฌธ์ if๋ฅผ ์ฌ์ฉํ์!

```tsx
const head = document.querySelector('#head')!;
console.log(head);

const head = document.querySelector('#head');
if (head) {
  console.log(head);
}
```

๐ย  string๊ณผ String์ ๋ค๋ฅด๋ค.

String์ ๋นํธ์ธ ๊ฐ์ฒด์ด๋ค. ๋ฐ๋ผ์ ๋์ ๋ค๋ฅด๋ค.

```tsx
const a: string = 'hello'; // O
const b: String = 'hell'; // X
```

๐ย  ํํ๋ฆฟ ๋ฆฌํฐ๋ด ํ์์ด ์กด์ฌํ๋ค.

```tsx
type World = 'world' | 'hell';

// type Greetting = 'hello world'
type Gretting = `hello ${World}`;
```

๐ย  ๋ฐฐ์ด, ํํ ๋ฌธ๋ฒ

```tsx
let arr: string[] = [];
let arr2: Array<string> = [];
function rest(...args: string[]) {}

const tuple: [string, number] = ['1', 1];
tuple[2] = 'hello'; // ์๋ฌ!
tuple.push('hello'); // ์ด๊ฑด tsc์์ ์ก์์ฃผ์ง ๋ชปํ๋ค ใ ใ  ์ด๋ฐ ๋ฐ๋ณด๊ฐ์!
```

๐ย  enum, keyof, typeof

enum์ ์๋ฐ๋ฅผ ํ์ ๋ ์ ๊น ์ฌ์ฉํด๋ณธ ๊ฒ ๊ฐ๋ค. enum์ ์ฌ์ฉํ๊ธฐ ์ซ๋ค๋ฉด keyof, typeof๋ฅผ ์ฌ์ฉํ๋ฉด ๋๋ค.

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

// keyof๋ ๊ฐ์ฒด ํ์์์ key๊ฐ์ ๊ฐ์ ธ์จ๋ค.
// typeof๋ ๊ฐ์ฒด ํ์์ ๋ํ๋ธ๋ค.
type Direction = typeof ODirection[keyof typeof ODirection];
function run(dir: Direction) {}

walk(EDirection.Left);
run(ODirection.Right);
```

๐ย  union

union์ โ๋๋โ์ด๋ผ ์๊ฐํ๋ฉด ๋๋ค. ์ฌ๋ฌ ๊ฐ ์ค ์ด๋ค ํ ๊ฐ์ด ๋ค์ด์ฌ ์ ์๋ค๋ ๋ป์ด๋ค.

```tsx
function add(x: string | number, y: string | number): string | number {
  return x + y;
}
add(1, 2);
add('1', '2');
add(1, '2');
```

๐ย  intersection๊ณผ ๊ฐ์ฒด์ ์์ฌ ์์ฑ ๊ฒ์ฌ

๋ ์์ธํ ๋ด์ฉ์ ์ฌ๊ธฐ๋ฅผ ๋๋ฌ ์ดํด๋ณด์.

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

๐ย  interface๋ผ๋ฆฌ๋ ์๋ก ํฉ์ณ์ง๋ค.

```tsx
interface A {
  a: string;
}
interface B {
  b: string;
}
const obj1: A = { a: 'hello', b: 'world' };

// type์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค!
type B = { a: string };
type B = { b: string };
const obj2: B = { a: 'hello', b: 'world' };
```

๐ย  void ํ์์ return ๊ฐ์ ์ฌ์ฉํ์ง ์๊ฒ ๋ค๋ ๋ป์ด๋ค.

๋ค๋ง ๋ฉ์๋๋ ๋งค๊ฐ๋ณ์์์๋ return ๊ฐ์ ์ฌ์ฉ ๊ฐ๋ฅํ๋ค๊ณ  ํ๋ ์กฐ์ฌํด์ผ ํ๋คโฆ?

๋ ์์ธํ ๋ด์ฉ์ [์ฌ๊ธฐ](https://www.notion.so/TS-void-0ebcce8446ba4ed7afe9ac433dc6a01f)๋ฅผ ํตํด ์ดํด๋ณด์!

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

๐ย  ํ์๋ง ์ ์ธํ๊ณ  ์ถ์ ๋ declare๋ฅผ ์ ์ธํ๋ค.

๋ค๋ง ๊ตฌํ์ ๋ค๋ฅธ ํ์ผ์ ์์ด์ผ ํ๋ค.

์๋ฅผ ๋ค์ด, ๋ธ๋ผ์ฐ์ ์์ ์ฌ๋ฌ script๋ฅผ ํตํด ๋ชจ๋์ ๊ฐ์ ธ์ค๋๋ฐ,๋ค๋ฅธ ๋ชจ๋์ ๊ตฌํ์ฒด๊ฐ ์๋ค๋ฉด ์ด๋ฅผ ์ด์ฉํ๋๊ฒ ์ ์ผ ๋ฒ ์คํธ์ด๋ค.

ํ๋ ์ด๋ฅผ tsc๊ฐ ์ ๋ฐฉ๋ฒ์ด ์๋ค. ํ์ง๋ง declare๋ฅผ ํตํด ๋ค๋ฅธ ๋ชจ๋์ ๊ตฌํ์ฒด๊ฐ ์๋ค๋ ๊ฒ์ tsc์ ์๋ฆด ์ ์๋ค.

```tsx
declare const a: string;
declare function a(x: number): number;
declare class A {}
```

๐ย  any์ unknown์ ์ฐจ์ด์ !

- any๋ tsc๊ฐ ํ์๊ฒ์ฌ๋ฅผ ํฌ๊ธฐํ๋ค.
- unknown์ ์ฌ์ฉ์๊ฐ ์ง์  ํ์์ ๋ฃ์ด์ค์ผ ํ๋ค.

๐ย  ํ์ ๊ฐ๋

TS๋ ํ์ ๊ตฌ๋ถ์ ์ ํด์ค๋ค!
์์ธํ ๋ด์ฉ์ [์ฌ๊ธฐ](https://www.notion.so/e605d9bf602d479cb9b23e64893c9a0f)๋ฅผ ์ฐธ๊ณ ํ์!

๐ย  {}์ Object ํ์

{}๊ณผ Object๋ null๊ณผ undefined๋ฅผ ์ ์ธํ ๋ชจ๋  ํ์์ ๋ปํ๋ค.

object๊ฐ ๊ฐ์ฒด๋ฅผ ๋ปํ๊ณ , Object๋ ๋ชจ๋  ํ์์ ๋ปํ๋ค.

๐ย  readonly

```tsx
interface A {
  readonly a: string;
  b: string;
}

const aaa: A = { a: 'hello', b: 'world' };

// ์๋ฌ ๋ฐ์!
// readonly ๋๋ฌธ์!
aaa.a = '123';
```

๐ย  index signiture

```tsx
// ๋ชจ๋  ๊ฐ์ฒด์ ์์ฑ์ key ๊ฐ๊ณผ value ๊ฐ์ ์ ์ํ ๊ฐ์ผ๋ก ๊ณ ์ ํ๋ค.
type A = { [key: string]: string };

const aaa: A = { a: 'hello', b: 'world' };

// ์๋ฌ ๋ฐ์!
// key์ value๊ฐ string์ด ์์ผํ๊ธฐ ๋๋ฌธ์!
const bbb: A = { a: 123 };
```

๐ย  mapped types

```tsx
// B์ ์ ์ํ ๊ฐ๋ค์ key ๊ฐ์ผ๋ก ๋ฌด์กฐ๊ฑด ํฌํจํด์ผ ํ๋ค!
type B = 'Human' | 'Mammal' | 'Animal';
type A = { [key in B]: number };

const aaa: A = { Human: 1, Mammal: 2, Animal: 3}

---

// value์ ๊ฐ๋ ์ ํํ  ์ ์๋ค.
type B = 'Human' | 'Mammal' | 'Animal';
type A = { [key in B]: B };

const aaa: A = {
	Human: 'Animal',
	Mammal: 'Human',
	Animal: 'Mammal',
}
```

๐ย  TS์์์ class

private, protected, abstract class ๋ฑ์ ๊ฐ๋์ด ์ถ๊ฐ๋์๋ค.

์์ธํ ์ฌํญ์ [์ฌ๊ธฐ](https://www.notion.so/TS-class-29b371a5b654432ea2ddc12c383ee0cc)์์ ์ดํด๋ณด์.

๐ย  optional์ ์์ด๋ ๋๊ณ , ์์ด๋ ๋๋ค๋ ์๋ฏธ๋ฅผ ๋ํ๋ธ๋ค.

```tsx
function abc(a: number, b?: number, c?:number){}

abc(1)
abc(1, 2)
abc(1, 2, 3)

// ์๋ฌ ๋ฐ์!!
abc(1, 2, 3, 4)

---

function abc(...args: number[]){}

abc(1)
abc(1, 2)
abc(1, 2, 3)

// ์ฑ๊ณต์ ์ผ๋ก ์ฌ๋ฌ ๊ฐ์ ์ธ์๋ฅผ ๋ฐ์์ฌ ์ ์๋ค.
abc(1, 2, 3, 4)
```

๊ฒฐ๊ตญ optional์ ๋งค๊ฐ๋ณ์๋ฅผ ์ ํํ๊ฒ ์ปจํธ๋กค ํ๋ ๋ฐ์ ์ด์ฉํ๋ค.

๐ย  [์ ๋๋ฆญ](https://www.notion.so/Generic-9a61f673c12b42d489c30e141f2fa317)

๐ย  ๊ธฐ๋ณธ๊ฐ ํ์ดํ

`๋งค๊ฐ๋ณ์: ํ์ = ๊ธฐ๋ณธ๊ฐ` ์ ํํ์ด๋ค.

์ ๋๋ฆญ์ ๊ธฐ๋ณธ๊ฐ์ ๋ฃ์ด์ฃผ๋๊ฒ ์ข๋คโฆ?
<T = unknown> ๊ฐ์ด ์ฌ์ฉ๋  ๋ ๋ฎ์ด์ฐ๊ธฐ๋ฅผ ํด์ ์๊ด์๋ค!
