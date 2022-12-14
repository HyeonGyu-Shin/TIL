<br>

## ๐ย  TS ๊ณต์ ๋ฌธ์ - Doโs and Donโt

<br>

### โ๏ธย  ์ผ๋ฐ ํ์(General Types)

<br>

๐ย  Number, String, Boolean, Symbol, Object์ ๊ฐ์ Built-in ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํ๋ฉด ์ ๋๋ค!

๋์  number, string, boolean, symbol, null or undefined, object์ ๊ฐ์ TS์์ ์ ๊ณตํด์ฃผ๋ ๋นํธ์ธ ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํด์ผ ํ๋ค.

<br>

```tsx
function reverse(s: String): String; // X
function reverse(s: string): string; // O
```

<br>

### โ๏ธย  ์ ๋๋ฆญ

<br>

๐ย  ํ์ ๋งค๊ฐ๋ณ์๋ฅผ ์ฌ์ฉํ์ง ์๋ ์ ๋๋ฆญ ํ์์ ์ฌ์ฉํ๋ฉด ์ ๋๋ค.

<br>

### โ๏ธย  ์ฝ๋ฐฑ์ ๋ฐํ ํ์(Return Types of Callbacks)

<br>

๐ย  ์ฌ์ฉํ์ง ์๋ ์ฝ๋ฐฑ์ return ํ์์ any๋ฅผ ์ฌ์ฉํ๋ฉด ์ ๋๋ค.

<br>

```tsx
// ์๋ชป๋จ!
function fn(x: () => any) {
  x();
}
```

<br>

๐ย  ์ฌ์ฉํ์ง ์๋ ์ฝ๋ฐฑ์ return ํ์์ void๋ฅผ ์ฌ์ฉํด์ผ ํ๋ค.

์๋? void๋ฅผ ์ฌ์ฉํ๋ฉด ์ค์๋ก x์ return ๊ฐ์ ์ฌ์ฉํ๋ ๊ฑธ ๋ฐฉ์งํ  ์ ์๊ธฐ ๋๋ฌธ์ด๋ค.

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

### โ๏ธย  ์ฝ๋ฐฑ์์ ์ ํ์  ๋งค๊ฐ๋ณ์(Optional Parameters in Callbacks)

<br>

๐ย  ์ ๋ง ์๋ํ ๊ฒ์ด ์๋๋ผ๋ฉด ์ฝ๋ฐฑ์ optional์ ์ฌ์ฉํ๋ฉด ์ ๋๋ค!

<br>

```tsx
// ์๋ชป๋จ
interface Fetcher {
  getObject(done: (data: any, elapsedTime?: number) => void): void;
}
```

<br>

์์ ํจ์๋ ๊ตฌ์ฒด์ ์ธ ์๋ฏธ๋ฅผ ๋๋ค. done ์ฝ๋ฐฑ์ 1๊ฐ ํน์ 2๊ฐ์ ์ธ์๋ก ํธ์ถ๋  ์ ์๋ค. ์๋ง elapsedTime ๋งค๊ฐ๋ณ์๊ฐ ์ฝ๋ฐฑ๊ณผ์ ์๊ด์ด ์๋ค๋ ๊ฑธ ๋ํ๋ด๋ ค๋ ์๋์์ ๊ฒ์ด๋ค.

ํ์ง๋ง ์ด๋ฌํ ์๋๋ฅผ ๋ง๋ค๊ธฐ ์ํด ๋งค๊ฐ๋ณ์๋ฅผ optional๋ก ๋ง๋ค ํ์๋ ์๋ค. ์๋๋ฉด ์ ์ํ ์ธ์๋ณด๋ค ๋ ์ ์ ์ธ์๋ฅผ ์ฝ๋ฐฑ์ ์ ๊ณตํ๋ ๊ฑด ํญ์ ํ์ฉ๋๊ธฐ ๋๋ฌธ์ด๋ค.

<br>

```tsx
interface Fetcher {
  getObject(done: (data: any, elapsedTime: number) => void): void;
}
```

<br>

### โ๏ธย  ์ค๋ฒ๋ก๋์ ์ฝ๋ฐฑ(Overloads and Callbacks)

<br>

๐ย  ์ฝ๋ฐฑ์ ์ธ์๋ง ๋ค๋ฅธ ์ค๋ฒ๋ก๋๋ฅผ ๋ถ๋ฆฌํด์ ์์ฑํ๋ฉด ์ ๋๋ค!

<br>

```tsx
// ์๋ชป๋จ
declare function beforeAll(action: () => void, timeout?: number): void;
declare function beforeAll(
  action: (done: DoneFn) => void,
  timeout?: number
): void;
```

<br>

์ต๋ ์ธ์๋ฅผ ์ฌ์ฉํด ํ๋์ ์ค๋ฒ๋ก๋๋ฅผ ์์ฑํด์ผ ํ๋ค.

<br>

```tsx
declare function beforeAll(
  action: (done: DoneFn) => void,
  timeout?: number
): void;
```

<br>

์ฝ๋ฐฑ์ด ๋งค๊ฐ๋ณ์๋ฅผ ๋ฌด์ํ๋ ๊ฑด ํญ์ ํ์ฉ๋จ์ผ๋ก, ๊ตณ์ด ์งง์ ์ค๋ฒ๋ก๋๋ฅผ ํ์๋ก ํ์ง ์๋๋ค. ๋ํ, ๋ ์งง์ ์ฝ๋ฐฑ์ ๋จผ์  ์์ฑํ๋ฉด ๋์ด์ค๋ ํจ์๊ฐ ์ฒซ ๋ฒ์งธ ์ค๋ฒ๋ก๋์ ์ผ์นํ๊ธฐ ๋๋ฌธ์ ์๋ชป๋ ํ์์ ํจ์๋ฅผ ํ์ฉํ  ์ ์๋ค.

<br>

### โ๏ธย  ํจ์ ์ค๋ฒ๋ก๋ ์์(Function Overloads Ordering)

<br>

๐ย  ๋ ๋์ ๋ฒ์์ ์ค๋ฒ๋ก๋๋ฅผ ๋ ์ข์ ๋ฒ์์ ์ค๋ฒ๋ก๋ ์ด์ ์ ๋๋ฉด ์๋๋ค. ์ฆ, ์ข์ ๋ฒ์ โ ๋์ ๋ฒ์์ ์์๋ก ์์ฑํด์ผ ํ๋ค.

<br>

```tsx
/* ์๋ชป๋จ */
declare function fn(x: any): any;
declare function fn(x: HTMLElement): number;
declare function fn(x: HTMLDivElement): string;

var myElem: HTMLDivElement;
var x = fn(myElem); // x: any, wat?
```

<br>

```tsx
/* ์ข์ */
declare function fn(x: HTMLDivElement): string;
declare function fn(x: HTMLElement): number;
declare function fn(x: any): any;

var myElem: HTMLDivElement;
var x = fn(myElem); // x: string, :)
```

<br>

์๋ํ๋ฉด TS๋ ํจ์ ํธ์ถ์ ์ฒ๋ฆฌํ  ๋ ์ฒซ ๋ฒ์งธ๋ก ์ผ์นํ๋ ์ค๋ฒ๋ก๋๋ฅผ ์ ํํ๋ค. ๋์ ๋ฒ์์ ์ค๋ฒ๋ก๋๊ฐ ์์ ์๋ค๋ฉด ์์ ์ค๋ฒ๋ก๋๋ค์ ๊ฐ๋ ค์ ธ์ ํธ์ถ๋์ง ์๋๋ค.

<br>

### โ๏ธย  ์ ํ์  ๋งค๊ฐ๋ณ์ ์ฌ์ฉ(Use Optional Parameters)

<br>

๐ย  ๋ค๋ฐ๋ผ์ค๋ ๋งค๊ฐ๋ณ์๋ง ๋ค๋ฅธ ์ค๋ฒ๋ก๋๋ฅผ ์์ฑํ๋ฉด ์ ๋๋ค.

<br>

```tsx
/* ์๋ชป๋จ */
interface Example {
  diff(one: string): number;
  diff(one: string, two: string): number;
  diff(one: string, two: string, three: boolean): number;
}
```

<br>

๊ฐ๋ฅํ ์ ํ์  ๋งค๊ฐ๋ณ์๋ฅผ ์ฌ์ฉํด์ผ ํ๋ค.

<br>

```tsx
/* ์ข์ */
interface Example {
  diff(one: string, two?: string, three?: boolean): number;
}
```

<br>

### โ๏ธย  ์ ๋์ธ ํ์ ์ฌ์ฉ (Use Union Types)

<br>

๐ย  ํ ์ธ์ ์์น์์ ํ์๋ง ๋ค๋ฅธ ์ค๋ฒ๋ก๋๋ฅผ ์ฌ์ฉํ๋ฉด ์ ๋๋ค.

<br>

```tsx
/* ์๋ชป๋จ */
interface Moment {
  utcOffset(): number;
  utcOffset(b: number): Moment;
  utcOffset(b: string): Moment;
}
```

<br>

๊ฐ๋ฅํ ์ ๋์ธ ํ์์ ์ฌ์ฉํด์ผ ํ๋ค.

<br>

```tsx
/* ์ข์ */
interface Moment {
  utcOffset(): number;
  utcOffset(b: number | string): Moment;
}
```

<br>

## ๐ย  ์ ๋ฆฌ

<br>

๐ย  ์ค๋ฒ๋ก๋๋ฅผ ํ  ๋ฐ์ optional์ ์ฌ์ฉํ๊ณ , ๋๋๋ก์ด๋ฉด optional์ ์ฌ์ฉํ์ง ๋ง์!
