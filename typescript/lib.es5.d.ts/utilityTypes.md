<br>

# Ts의 Utility types에 대해 알아보자.

<br>

## ✏️  Partial

<br>

```tsx
interface Profile {
  name: string;
  age: number;
  married: boolean;
}

const me: Profile = {
  name: 'shin',
  age: 27,
};
```

<br>

👉  결혼 정보는 공개하지 않아도 된다고 했을 때, 위의 코드는 에러가 난다. 왜냐하면 Profile의 모든 요소를 가지고 있어야하기 때문이다.

그럼 어떻게 해야할까?

<br>

```tsx
interface Profile {
  name: string;
  age: number;
  married?: boolean;
}

const me: Profile = {
  name: 'shin',
  age: 27,
};
```

<br>

optional을 이용하여 에러는 해결했지만 Profile 인터페이스를 재사용할 순 없게 되었다.

인터페이스를 하나 더 만드는게 해결책인데, 이 방식은 너무 비효율적이다.

이럴 때 사용하는게 utility type이고, 위의 경우에는 Partial이다.

<br>

```tsx
interface Profile {
  name: string;
  age: number;
  married: boolean;
}

const me: Partial<Profile> = {
  name: 'shin',
  age: 27,
};
```

<br>

<aside>
💡 하지만 Partial은 모든 매개변수를 optional로 만들어주기 때문에 되도록 사용하지 않는게 좋다!

</aside>

<br>

## ✏️  Pick

<br>

👉  Pick type은 두 번째에 정의한 타입을 첫 번째 타입에서 가져와주는 utility Type이다.

<br>

```tsx
// Profile이라는 설계도를 만들었다.
// {name, age, isMarried} 속성을 꼭 가지고 있어야 한다.
interface Profile {
  name: string;
  age: number;
  isMarried: boolean;
}

// 설계도에 따라 객체를 만들었다.
const shin: Profile = {
  name: 'shin',
  age: 27,
  isMarried: false,
};

// Pick Utility type을 만들어봤다.
// K는 T 객체의 key 값들이어야 한다.
type P<T, K extends keyof T> = {
  // K를 순회하면서 새로운 설계도를 만들어준다.
  [Key in K]: T[Key];
};

// Profile 설계도에서 name과 age만을 포함하는 새로운 설계도를 토대로
// 새로운 객체를 만들었다.
const newShin: P<Profile, 'name' | 'age'> = {
  name: 'shin',
  age: 27,
};
```

<br>

👉  공식문서에 나와있는 Pick utility type을 살펴보자.

<br>

```tsx
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

<br>

위에서 정의한 P 타입과 같은 모습을 볼 수 있다.

<br>

## ✏️  Exclude

<br>

👉  Omit에 대해 알아보려고 했는데, Omit에 대해 알기 위해서는 Exclude를 알아야하기 때문에 Exclude에 대해 알아보자.

<br>

```tsx
type Exclude<T, U> = T extends U ? never : T;
```

<br>

T와 U가 같다면 사라지고, 그렇지 않으면 T를 반환해준다. 이렇게만 보면 이해하기 어렵기 때문에 예제를 통해 알아보자.

<br>

```tsx
interface Profile {
  name: string;
  age: number;
  isMarried: boolean;
}

// Profile은 객체이기 때문에 keyof를 통해 프로퍼티들을
// 'name' | 'age' | 'isMarried' 형식으로 변환해준다.
// 자 이제 Exclude를 해석해보자.
// T = 'name' | 'age' | 'isMarried;
// U = 'isMarried'
// T를 순회하면서 U와 같다면 never를, 다르다면 T를 return해준다.
type A = Exclude<keyof Profile, 'isMarried'>;

// 그 결과 A에는 'name' | 'age'가 나오게 된다.
```

<br>

Exclude의 결과 값은 Union 타입이기 때문에 다른 utility types에 이용하는 것 같다. 아 물론 그냥도 사용할 순 있겠지만 지금은 떠오르는 예시가 없어서 다루진 않겠다.

<br>

## ✏️  Omit

<br>

👉  이번엔 Omit utility type을 살펴보자.

<br>

```tsx
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
```

<br>

Pick과 Exclude가 사용된 모습을 볼 수 있다. 천천히 뜯어보면서 알아보자.

Exclude를 통해 필터링된 유니온 타입을 가져온 후, Pick을 통해 union 값을 포함하는 새로운
객체를 반환한다.

<br>

```tsx
interface Profile {
  name: string;
  age: number;
  isMarried: boolean;
}

// Omit의 결과 두 번째로 들어오는 값을 제외한 새로운 객체 타입이 return 된다.
const newShin: Omit<Profile, 'isMarried'> = {
  name: 'shin',
  age: 27,
};
```

<br>

그런데 궁금증이 생겼다.

<br>

```tsx
interface Profile {
  name: string;
  age: number;
  isMarried: boolean;
}

// O의 두 번째 제너릭에 extends를 이용하여 타입 가드를 하지 않아도 동작한다.
// type O<T, S> = Pick<T, Exclude<keyof T, S>>

// 타입 가드를 위해선 아래와 같이 사용되는게 맞지 않나? 라는 생각을 했다.
// 그런데 아마도 혹시나 객체에 들어가면 안되는 값이 있다면 그 값을 지우기 위해서라는
// 뉘앙스를 위해 any를 사용하는 것 같다.
// type O<T, S extends T> = Pick<T, Exclude<keyof T, S>>

// type O<T, S extends any> = Pick<T, Exclude<keyof T, S>>

// 그 결과 money라는 민감한 속성이 있다면 없애고
// 새로운 객체 타입을 반환해준다.
const newShin: O<Profile, 'money'> = {
  name: 'shin',
  age: 27,
  isMarried: false,
};
```

<br>

## 🔎  Readonly

<br>

👉  Readonly 수정을 불가능하게 해주는 utility type이다.

<br>

```tsx
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

<br>

readonly를 붙여줘서 속성이 변화하면 에러를 발생시키도록 한다.

<br>

```tsx
interface Profile {
  name: string;
  age: number;
  isMarried: boolean;
}

type R<T> = {
  readonly [Key in keyof T]: T[Key];
};

const newShin: R<Profile> = {
  name: 'shin',
  age: 27,
  isMarried: false,
};

// 에러 발생!
// readonly가 속성에 붙어있기 때문에 수정 불가능!
newShin.name = 'shishin';
```

<br>

## 🔎  Required

<br>

👉  Required는 제너릭 안의 모든 속성에 있는 optional을 제거해주는 utility type이다.

<br>

```tsx
type Required<T> = {
  [P in keyof T]-?: T[P];
};
```

<br>

-?는 optional을 제거해주는 modify 함수(?)라고 생각하면 된다. 이를 통해 제너릭 안의 속성들의 optional을 제거한 새로운 객체 타입을 반환해준다.

<br>

```tsx
interface Profile {
  name?: string;
  age?: number;
  isMarried?: boolean;
}

type R<T> = {
  [Key in keyof T]-?: T[Key];
};

// optional을 제거했기 때문에
// isMarried가 없다는 에러가 발생!!
const newShin: R<Profile> = {
  name: 'shin',
  age: 27,
  // isMarried: false,
};
```

<br>

## 🔎  Record

<br>

👉  Record는 객체 프로퍼티의 타입을 쉽게 정의할 수 있도록 도와주는 utility type이다.

<br>

```tsx
type Record<K extends keyof any, T> = {
  [P in K]: T;
};
```

<br>

예시를 살펴보자.

<br>

```tsx
interface Obj1 {
  [key: string]: number;
}

type Obj2 = {
  [key: string]: number;
};

// Record 자리에 Obj1, Obj2를 넣어도 괜찮다.
const a: Record<string, number> = {
  a: 3,
  b: 5,
};
```

<br>

객체 타입을 여러 방법을 이용하여 정의했다. 이를 통해 Record에 대한 이해를 더 쉽게 할 수 있었다.

<br>

```tsx
interface Profile  {
    nam: string,
    age: number,
    isMarried?: boolean,
}

type R<K extends keyof any, T> = {
    [P in K]:T
}

// age와 isMarried에서 에러가 발생한다.
// newShin의 객체는 string, string 타입 쌍이기 때문이다.
const newShin:R<string,string> = {
    name:'shin',
    age: 27,
    isMarried: false,
}

---

// 따라서 에러를 해결하려면
// 아래와 같이 모든 프로퍼티를 string으로 변경하면 된다.
const newShin:R<string,string> = {
    name:'shin',
    age: '27',
    isMarried: 'false',
}
```

<br>

## 🔎  NonNullable

<br>

👉  NonNullable은 key types에서 null | undefined를 없애주는 utility type이다.

<br>

```tsx
type NonNullable<T> = T & {};
// type NonNullable<T> = T extends null | undefined? never : T
```

<br>

공식 문서에는 간략하게 나와있어서 좀 더 풀어서 작성을 했다.

<br>

```tsx
type A = string | number | null | undefined | boolean;

// B = string | number | boolean
type B = NonNullable<A>;
```

<br>

A에서 작성한 key types에서 null과 undefined가 제외된 나머지 값들만 반환되었다.

<br>

## ✏️  Parameters

<br>

👉  Parameters는 제너릭에 함수를 받아와서 그 함수의 parameter를 가져와주는 utility type이다.

 <br>

```tsx
type Parameters<T extends (...args: any) => any> = T extends (
  ...args: infer P
) => any
  ? P
  : never;
```

<br>

infer에 대한 건 [여기](https://www.notion.so/Infer-e4ce7d753ee245999b9d9fe2a146028d)에서 더 자세히 살펴보도록 하자.
아직 위의 코드만 보면 이해하기 어려우니 예시를 통해 더 알아보도록 하자.

<br>

```tsx
// 매개변수가 x: number, y: string, z: boolean인 함수 정의
function zip(
  x: number,
  y: string,
  z: boolean
): { x: number; y: string; z: boolean } {
  return { x, y, z };
}

// Parameters를 직접 구현해봤다.
// 함수를 받아와서 그 함수가 유효하면 해당 함수의 매개변수인 U를 return 해준다.
type P<T extends (...args: any) => any> = T extends (...args: infer U) => any
  ? U
  : never;

// [x: number, y: string, z: boolean]가 return 되었다.
type params = P<typeof zip>;
```

<br>

함수가 성공적으로 들어왔기 때문에 U에 함수의 매개변수가 할당이 되고, U가 반환되어 params에 함수의 parameter들이 성공적으로 할당되었다.

<br>

## ✏️  ReturnType

<br>

👉  ReturnType도 Parameters와 비슷하게 infer를 사용하여 함수의 return 값을 반환해주는 utility type이다.

<br>

```tsx
type ReturnType<T extends (...args: any) => any> = T extends (
  ...args: any
) => infer R
  ? R
  : any;
```

<br>

parameters에서 처럼 함수를 받아와서 infer를 통해 함수의 return 값을 가져와서 가져온return 값을 반환해준다.

<br>

```tsx
// return의 타입 값이 {x: number, y: string, z: boolean}인 함수 정의
function zip(
  x: number,
  y: string,
  z: boolean
): { x: number; y: string; z: boolean } {
  return { x, y, z };
}

// 함수를 받아와서 함수가 유효하면 함수의 return 타입 값을 가져와서 반환해준다.
type R<T extends (...args: any) => any> = T extends (...args: any) => infer R
  ? R
  : never;

// {x: number, y: string, z: boolean}를 반환해준다.
type params = R<typeof zip>;
```

<br>

## ✏️  ConstructorParameters & InstanceType

<br>

👉  위의 두 가지 utility types는 class와 관련이 있다.

<br>

```tsx
type ConstructorParameters<T extends abstract new (...args: any) => any> =
  T extends abstract new (...args: infer P) => any ? P : never;
type InstanceType<T extends abstract new (...args: any) => any> =
  T extends abstract new (...args: any) => infer R ? R : any;
```

<br>

둘 다 생성자 함수를 받아온 후 각각 생성자 함수의 매개변수와 Return 값을 반환해준다.

<br>

```tsx
class A {
  a: string;
  b: number;
  constructor(a: string, b: number) {
    this.a = a;
    this.b = b;
  }
}

// 직접 ConstructorParameters를 구현
type CP<T extends abstract new (...args: any) => any> = T extends abstract new (
  ...args: infer U
) => any
  ? U
  : never;

// C1과 C2 모두 [a: string, b: number]를 반환
type C1 = CP<typeof A>;
type C2 = ConstructorParameters<typeof A>;

// 직접 InstanceType 구현
type I<T extends abstract new (...args: any) => any> = T extends abstract new (
  ...args: any
) => infer U
  ? U
  : never;

// I1과 I2 모두 A를 반환한다.
type I1 = I<typeof A>;
type I2 = InstanceType<typeof A>;
```

<br>
