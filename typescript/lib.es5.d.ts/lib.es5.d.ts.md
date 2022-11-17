<br>

# TS lib.es5.d.ts 분석

<br>

## 🔎  lib.es5.d.ts 분석

<br>

### ✏️  forEach

<br>

👉  JS의 forEach 함수는 배열을 순회하는 메서드이다.

다음은 forEach 함수를 추상화한 인터페이스이다.

<br>

```tsx
interface Array<T> {
  forEach(
    callbackfn: (value: T, index: number, array: T[]) => void,
    thisArg?: any
  ): void;
}
```

forEach는 콜백함수를 받아서 순회를 한다.

callbackfn이 콜백함수에 해당한다. callbackfn은 value, index, array의 매개변수를 받고, 아무런 값도 return하지 않는다는 기능을 잘 설명해주고 있다.

<br>

### ✏️  map

<br>

👉  JS의 map 함수는 배열을 순회한 후 콜백함수의 결과를 배열에 담아 그 배열을 return 해주는 메서드이다.

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

map은 forEach와 다르게 return 값을 가진다.

따라서 U라는 제너릭을 하나 더 사용하여 처리된 결과를 담은 배열을 return 해주는 모습을 볼 수 있다.

<br>

### ✏️  filter

<br>

👉  filter 메서드는 두 가지의 메서드가 존재한다.

한 번 살펴보도록 하자.

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

위의 두 가지 중 한 개를 선택해서 구현하면 된다고 생각한다.
정확히는 아직 모르겠다…ㅠㅅㅠ

<br>

```tsx
interface Array<T>{
	filter<S extends T>(predicate: (value: T, index: number, array: T[]) => value is S, thisArg?: any): S[];
}

// 첫 번째 구현체를 선택한 경우이다.
// 이 경우, 커스텀 타입 가드를 통해 return 값을 정해줘야 한다.
// value에 string이 들어간다고 선언했으니 result는 string[]이 된다.
const predicate = (value:string| number): value is string => typeof value === 'string';
const result = ['1', 2, '3', 4, '5'].filter(predicate)

---

interface Array<T>{
	filter(predicate: (value: T, index: number, array: T[]) => unknown, thisArg?: any): T[];
}

// 두 번째 구현체를 선택한 경우이다.
// 이 경우, 커스텀 타입 가드를 하지 않고, 배열에 들어오는 타입으로
// return의 타입을 정한다.
const predicate = (value: string|number) => typeof value === 'string';
const result = ['1', 2, '3', 4, '5'].filter(predicate)
```

<br>

### ✏️  forEach 타입 직접 구현해보기

<br>

```tsx
interface Arr {
  forEach(): void;
}

const arr: Arr = [1, 2, 3];

arr.forEach();
```

<br>

👉  기존에 있는 forEach 메서드를 커스텀한 타입을 만들어서 구현해보려고 한다.

<br>

<img src='./images/untitled.png'/>

<br>

👉  아직 타입을 구현하지 않았기에 타입이 맞지 않다는 오류가 나온다.

<br>

```tsx
interface Arr {
  forEach(callbackfn: (value: number) => void): void;
}

const arr: Arr = [1, 2, 3];

arr.forEach((item) => console.log(item));
```

<br>

👉  메서드를 채워주면 에러가 모두 해결된다.

여기서 끝!! …. 이 아니라 다른 타입도 들어올 수 있도록 확장성 있게 만들어야 한다.

<br>

<img src='./images/Untitled 1.png'/>

<br>

👉  이번엔 문자열 배열에 Arr 타입을 적용하니 오류가 발생한다.

이를 해결하기 위해 다음과 같이 바꿨다.

<br>

<img src='./images/Untitled 2.png'/>

<br>

하지만 문제가 있다. 제너릭을 배울 때도 언급했지만, item은 모든 가능성을 가지고 있기 때문에 string 또는 number의 다른 메서드는 사용하지 못한다.

따라서 제너릭을 사용해야 한다.

<br>

<img src='./images/Untitled 3.png'/>

<br>

그 결과 원본과 거의 비슷하게 잘 정의했다.
처음엔 필요한 부분만 구현하고, 확장할 필요성이 생길 때마다 확장해나가면 된다!

<br>

### ✏️  map 타입 만들기

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

👉  위와 같이 map 타입을 만들었다. 하지만 다음과 같은 의문이 든다.

map의 callback에서 값의 타입을 바꾼다면…?

<br>

<img src='./images/Untitled 4.png'/>

<br>

에러가 발생한다. 이를 해결하기 위해서 제너릭을 한 개 더 사용해야 한다.

<br>

<img src='./images/Untitled 5.png'/>

<br>

제너릭을 한 개 더 추가하여 에러를 잡았고, 검증도 했다.

### ✏️  filter 타입 만들어보기

<br>

<img src='./images/Untitled 6.png'/>

<br>

👉  filter 타입은 생각을 더 많이 해봐야할 것 같다.

callbackfn의 return이 true인 원래의 값을 배열에 담아 return 해줘야 한다.

<br>

<img src='./images/Untitled 7.png'/>

<br>

return의 타입을 is를 이용하여 정해줬다. 그리고 구현체에서 return에 대해 정의를 해줘서 에러를 처리했다.

하지만 아직도 d에는 string | number 배열이라고 나온다.

<br>

<img src='./images/Untitled 8.png'/>

<br>

제너릭을 하나 더 사용하여 d의 타입은 원하던 string[]이 된다.

하지만 새로운 에러가 생겼다.

살펴보면 value는 T였는데, 어떻게 S가 되냐는 것이다. 즉, 연관관계가 없다는 뜻이다.

이를 해결하기 위해 S가 T의 부분집합이라는 관계를 명시해줘야 한다.

<br>

<img src='./images/Untitled 9.png'/>

<br>
