<br>

# TS lib.es5.d.ts ๋ถ์

<br>

## ๐ย  lib.es5.d.ts ๋ถ์

<br>

### โ๏ธย  forEach

<br>

๐ย  JS์ forEach ํจ์๋ ๋ฐฐ์ด์ ์ํํ๋ ๋ฉ์๋์ด๋ค.

๋ค์์ forEach ํจ์๋ฅผ ์ถ์ํํ ์ธํฐํ์ด์ค์ด๋ค.

<br>

```tsx
interface Array<T> {
  forEach(
    callbackfn: (value: T, index: number, array: T[]) => void,
    thisArg?: any
  ): void;
}
```

forEach๋ ์ฝ๋ฐฑํจ์๋ฅผ ๋ฐ์์ ์ํ๋ฅผ ํ๋ค.

callbackfn์ด ์ฝ๋ฐฑํจ์์ ํด๋นํ๋ค. callbackfn์ value, index, array์ ๋งค๊ฐ๋ณ์๋ฅผ ๋ฐ๊ณ , ์๋ฌด๋ฐ ๊ฐ๋ returnํ์ง ์๋๋ค๋ ๊ธฐ๋ฅ์ ์ ์ค๋ชํด์ฃผ๊ณ  ์๋ค.

<br>

### โ๏ธย  map

<br>

๐ย  JS์ map ํจ์๋ ๋ฐฐ์ด์ ์ํํ ํ ์ฝ๋ฐฑํจ์์ ๊ฒฐ๊ณผ๋ฅผ ๋ฐฐ์ด์ ๋ด์ ๊ทธ ๋ฐฐ์ด์ return ํด์ฃผ๋ ๋ฉ์๋์ด๋ค.

<br>

```tsx
interface Array<T> {
  map<U>(
    callbackfn: (value: T, index: number, array: T[]) => U,
    thisArg?: any
  ): U[];
}
```

<br>

map์ forEach์ ๋ค๋ฅด๊ฒ return ๊ฐ์ ๊ฐ์ง๋ค.

๋ฐ๋ผ์ U๋ผ๋ ์ ๋๋ฆญ์ ํ๋ ๋ ์ฌ์ฉํ์ฌ ์ฒ๋ฆฌ๋ ๊ฒฐ๊ณผ๋ฅผ ๋ด์ ๋ฐฐ์ด์ return ํด์ฃผ๋ ๋ชจ์ต์ ๋ณผ ์ ์๋ค.

<br>

### โ๏ธย  filter

<br>

๐ย  filter ๋ฉ์๋๋ ๋ ๊ฐ์ง์ ๋ฉ์๋๊ฐ ์กด์ฌํ๋ค.

ํ ๋ฒ ์ดํด๋ณด๋๋ก ํ์.

<br>

```tsx
interface Array<T> {
  filter<S extends T>(
    predicate: (value: T, index: number, array: T[]) => value is S,
    thisArg?: any
  ): S[];
  filter(
    predicate: (value: T, index: number, array: T[]) => unknown,
    thisArg?: any
  ): T[];
}
```

<br>

์์ ๋ ๊ฐ์ง ์ค ํ ๊ฐ๋ฅผ ์ ํํด์ ๊ตฌํํ๋ฉด ๋๋ค๊ณ  ์๊ฐํ๋ค.
์ ํํ๋ ์์ง ๋ชจ๋ฅด๊ฒ ๋คโฆใ ใใ 

<br>

```tsx
interface Array<T>{
	filter<S extends T>(predicate: (value: T, index: number, array: T[]) => value is S, thisArg?: any): S[];
}

// ์ฒซ ๋ฒ์งธ ๊ตฌํ์ฒด๋ฅผ ์ ํํ ๊ฒฝ์ฐ์ด๋ค.
// ์ด ๊ฒฝ์ฐ, ์ปค์คํ ํ์ ๊ฐ๋๋ฅผ ํตํด return ๊ฐ์ ์ ํด์ค์ผ ํ๋ค.
// value์ string์ด ๋ค์ด๊ฐ๋ค๊ณ  ์ ์ธํ์ผ๋ result๋ string[]์ด ๋๋ค.
const predicate = (value:string| number): value is string => typeof value === 'string';
const result = ['1', 2, '3', 4, '5'].filter(predicate)

---

interface Array<T>{
	filter(predicate: (value: T, index: number, array: T[]) => unknown, thisArg?: any): T[];
}

// ๋ ๋ฒ์งธ ๊ตฌํ์ฒด๋ฅผ ์ ํํ ๊ฒฝ์ฐ์ด๋ค.
// ์ด ๊ฒฝ์ฐ, ์ปค์คํ ํ์ ๊ฐ๋๋ฅผ ํ์ง ์๊ณ , ๋ฐฐ์ด์ ๋ค์ด์ค๋ ํ์์ผ๋ก
// return์ ํ์์ ์ ํ๋ค.
const predicate = (value: string|number) => typeof value === 'string';
const result = ['1', 2, '3', 4, '5'].filter(predicate)
```

<br>

### โ๏ธย  forEach ํ์ ์ง์  ๊ตฌํํด๋ณด๊ธฐ

<br>

```tsx
interface Arr {
  forEach(): void;
}

const arr: Arr = [1, 2, 3];

arr.forEach();
```

<br>

๐ย  ๊ธฐ์กด์ ์๋ forEach ๋ฉ์๋๋ฅผ ์ปค์คํํ ํ์์ ๋ง๋ค์ด์ ๊ตฌํํด๋ณด๋ ค๊ณ  ํ๋ค.

<br>

<img src='./images/untitled.png'/>

<br>

๐ย  ์์ง ํ์์ ๊ตฌํํ์ง ์์๊ธฐ์ ํ์์ด ๋ง์ง ์๋ค๋ ์ค๋ฅ๊ฐ ๋์จ๋ค.

<br>

```tsx
interface Arr {
  forEach(callbackfn: (value: number) => void): void;
}

const arr: Arr = [1, 2, 3];

arr.forEach((item) => console.log(item));
```

<br>

๐ย  ๋ฉ์๋๋ฅผ ์ฑ์์ฃผ๋ฉด ์๋ฌ๊ฐ ๋ชจ๋ ํด๊ฒฐ๋๋ค.

์ฌ๊ธฐ์ ๋!! โฆ. ์ด ์๋๋ผ ๋ค๋ฅธ ํ์๋ ๋ค์ด์ฌ ์ ์๋๋ก ํ์ฅ์ฑ ์๊ฒ ๋ง๋ค์ด์ผ ํ๋ค.

<br>

<img src='./images/Untitled 1.png'/>

<br>

๐ย  ์ด๋ฒ์ ๋ฌธ์์ด ๋ฐฐ์ด์ Arr ํ์์ ์ ์ฉํ๋ ์ค๋ฅ๊ฐ ๋ฐ์ํ๋ค.

์ด๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด ๋ค์๊ณผ ๊ฐ์ด ๋ฐ๊ฟจ๋ค.

<br>

<img src='./images/Untitled 2.png'/>

<br>

ํ์ง๋ง ๋ฌธ์ ๊ฐ ์๋ค. ์ ๋๋ฆญ์ ๋ฐฐ์ธ ๋๋ ์ธ๊ธํ์ง๋ง, item์ ๋ชจ๋  ๊ฐ๋ฅ์ฑ์ ๊ฐ์ง๊ณ  ์๊ธฐ ๋๋ฌธ์ string ๋๋ number์ ๋ค๋ฅธ ๋ฉ์๋๋ ์ฌ์ฉํ์ง ๋ชปํ๋ค.

๋ฐ๋ผ์ ์ ๋๋ฆญ์ ์ฌ์ฉํด์ผ ํ๋ค.

<br>

<img src='./images/Untitled 3.png'/>

<br>

๊ทธ ๊ฒฐ๊ณผ ์๋ณธ๊ณผ ๊ฑฐ์ ๋น์ทํ๊ฒ ์ ์ ์ํ๋ค.
์ฒ์์ ํ์ํ ๋ถ๋ถ๋ง ๊ตฌํํ๊ณ , ํ์ฅํ  ํ์์ฑ์ด ์๊ธธ ๋๋ง๋ค ํ์ฅํด๋๊ฐ๋ฉด ๋๋ค!

<br>

### โ๏ธย  map ํ์ ๋ง๋ค๊ธฐ

<br>

```tsx
interface Arr<T> {
  forEach(callbackfn: (value: T) => void): void;
  map(callbackfn: (value: T) => T): T[];
}

const numArr: Arr<number> = [1, 2, 3];
const result = numArr.map((value) => value * 2);
```

<br>

๐ย  ์์ ๊ฐ์ด map ํ์์ ๋ง๋ค์๋ค. ํ์ง๋ง ๋ค์๊ณผ ๊ฐ์ ์๋ฌธ์ด ๋ ๋ค.

map์ callback์์ ๊ฐ์ ํ์์ ๋ฐ๊พผ๋ค๋ฉดโฆ?

<br>

<img src='./images/Untitled 4.png'/>

<br>

์๋ฌ๊ฐ ๋ฐ์ํ๋ค. ์ด๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด์ ์ ๋๋ฆญ์ ํ ๊ฐ ๋ ์ฌ์ฉํด์ผ ํ๋ค.

<br>

<img src='./images/Untitled 5.png'/>

<br>

์ ๋๋ฆญ์ ํ ๊ฐ ๋ ์ถ๊ฐํ์ฌ ์๋ฌ๋ฅผ ์ก์๊ณ , ๊ฒ์ฆ๋ ํ๋ค.

### โ๏ธย  filter ํ์ ๋ง๋ค์ด๋ณด๊ธฐ

<br>

<img src='./images/Untitled 6.png'/>

<br>

๐ย  filter ํ์์ ์๊ฐ์ ๋ ๋ง์ด ํด๋ด์ผํ  ๊ฒ ๊ฐ๋ค.

callbackfn์ return์ด true์ธ ์๋์ ๊ฐ์ ๋ฐฐ์ด์ ๋ด์ return ํด์ค์ผ ํ๋ค.

<br>

<img src='./images/Untitled 7.png'/>

<br>

return์ ํ์์ is๋ฅผ ์ด์ฉํ์ฌ ์ ํด์คฌ๋ค. ๊ทธ๋ฆฌ๊ณ  ๊ตฌํ์ฒด์์ return์ ๋ํด ์ ์๋ฅผ ํด์ค์ ์๋ฌ๋ฅผ ์ฒ๋ฆฌํ๋ค.

ํ์ง๋ง ์์ง๋ d์๋ string | number ๋ฐฐ์ด์ด๋ผ๊ณ  ๋์จ๋ค.

<br>

<img src='./images/Untitled 8.png'/>

<br>

์ ๋๋ฆญ์ ํ๋ ๋ ์ฌ์ฉํ์ฌ d์ ํ์์ ์ํ๋ string[]์ด ๋๋ค.

ํ์ง๋ง ์๋ก์ด ์๋ฌ๊ฐ ์๊ฒผ๋ค.

์ดํด๋ณด๋ฉด value๋ T์๋๋ฐ, ์ด๋ป๊ฒ S๊ฐ ๋๋๋ ๊ฒ์ด๋ค. ์ฆ, ์ฐ๊ด๊ด๊ณ๊ฐ ์๋ค๋ ๋ป์ด๋ค.

์ด๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด S๊ฐ T์ ๋ถ๋ถ์งํฉ์ด๋ผ๋ ๊ด๊ณ๋ฅผ ๋ช์ํด์ค์ผ ํ๋ค.

<br>

<img src='./images/Untitled 9.png'/>

<br>
