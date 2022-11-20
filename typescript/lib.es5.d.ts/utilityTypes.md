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
