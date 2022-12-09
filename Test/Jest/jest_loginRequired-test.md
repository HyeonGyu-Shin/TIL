<br>

# 📌 &nbsp; Elice 1차 프로젝트에 Jest를 적용해보자!

<br>

👉 &nbsp; Elice 1차 프로젝트를 하면서 많은 것들을 느끼고 있다.

&nbsp; 프로젝트가 계속 진행되면서 스키마가 자꾸 변경되다 보니 기존에 만들어뒀던 서비스나 컨트롤러에 대한 신뢰성이 자꾸 떨어지는 문제가 발생했다.

&nbsp; 지금까진 수작업으로 Postman을 통해 API들을 체크해왔었다. 막연하게 테스트 코드도 짜봐야지라는 생각을 가지고 있었는데, Jest를 도입하면 좋겠다는 feedback을 받게 되었다.

&nbsp; 따라서 이번 기회에 Jest에 대해 배워보고 프로젝트에 적용해보려고 한다.

<br>

# 📌 &nbsp; Jest에 대해 배워보자!!

<br>

<br>

## 🔎 &nbsp; What is Jest...?

<br>

👉 &nbsp; Jest는 단순성을 중점에 둔 JS의 테스팅 프레임워크라고 공식문서에 나와있다. 바벨, TS, Node, React 등등에서 사용할 수 있다고 한다.

&nbsp; 테스트를 쉽게 하기 위해 만들어진 테스트 프레임워크라는 걸 알게되었다.

<br>

# 📌 &nbsp; Jest를 프로젝트에 적용해보자!

<br>

👉 &nbsp; CRUD에 먼저 적용을 하고싶었으나... DB와의 연관성 때문에 시행착오를 많이 겪었다...ㅠㅠ 따라서 로그인을 검사하는 미들웨어에 먼저 적용해보려고 한다.

```js
// loginRequired.js

import jwt from 'jsonwebtoken';
import { UnauthorizedError } from '../utils';

function loginRequired(req, res, next) {
  const userToken = req.headers['authorization']?.split(' ')[1];

  if (!userToken) {
    return next(
      new UnauthorizedError('로그인한 유저만 사용할 수 있는 서비스입니다.')
    );
  }

  try {
    const secretKey = process.env.JWT_SECRET_KEY || 'secret-key';
    const jwtDecoded = jwt.verify(userToken, secretKey);
    req.currentUserId = jwtDecoded.userId;

    next();
  } catch (error) {
    return next(new UnauthorizedError('정상적인 토큰이 아닙니다.'));
  }
}

export { loginRequired };
```

👉 &nbsp; 테스트를 짤 때는 분기를 기준으로 나누라고 들었다. 위의 미들웨어의 경우 3가지의 분기로 나눌 수 있다.

1. userToken이 존재하지 않는 경우

2. userToken이 존재하지만 정상적인 토큰이 아닌 경우

3. userToken이 존재하고 정상적인 토큰일 경우

&nbsp; 자 그럼 이제 각 분기별로 테스트 코드를 만들러 가보자!

<br>

### ✏️ &nbsp; 1. userToken이 존재하지 않는 경우

<br>

```js
// loginRequired.test.js

import { UnauthorizedError } from '../utils';
import { loginRequired } from './loginRequired';

test('req.headers.authorization이 없어서 에러를 던진다.', () => {
  const req = {
    headers: {},
  };
  const res = jest.fn();
  const next = jest.fn();

  loginRequired(req, res, next);

  expect(next).toBeCalledWith(
    new UnauthorizedError('로그인한 유저만 사용할 수 있는 서비스입니다.')
  );
});
```

👉 &nbsp; req.headers에 authorization 값이 없다. 따라서 userToken이 undefined이기 때문에 조건문에 걸려서 next(error 객체)를 실행하고 미들웨어를 종료한다.

&nbsp; 이를 검증하기 위해 next 함수가 `new UnauthorizedError('로그인한 유저만 사용할 수 있는 서비스입니다.')`를 인수로 호출되었는지 확인하는 코드를 작성했다.

<br>

### ✏️ &nbsp; 2. userToken이 존재하지만 정상적인 토큰이 아닌 경우

<br>

```js
// loginRequired.test.js

import { UnauthorizedError } from '../utils';
import { loginRequired } from './loginRequired';

test('req.headers.authorization은 있지만 정상적인 토큰이 아니다.', () => {
  const req = {
    headers: {
      authorization: 'bearerToken thisIsBearerToken',
    },
  };
  const res = jest.fn();
  const next = jest.fn();

  loginRequired(req, res, next);

  expect(next).toBeCalledWith(
    new UnauthorizedError('정상적인 토큰이 아닙니다.')
  );
});
```

👉 &nbsp; req.headers.authorization에 올바른 토큰 값이 들어왔기 때문에 userToken에 값이 존재하게 되고 if문을 통과한다. 하지만 try문에서 에러가 발생하기 때문에 next(error 객체)를 실행하고 미들웨어가 종료된다.

&nbsp; 이를 검증하기 위해 next 함수가 ` new UnauthorizedError('정상적인 토큰이 아닙니다.')`를 인수로 호출되었는지 확인하는 코드를 작성했다.

<br>

### ✏️ &nbsp; 3. userToken이 존재하고 정상적인 토큰일 경우

<br>

```js
jest.mock('jsonwebtoken');
import jwt from 'jsonwebtoken';
import { UnauthorizedError } from '../utils';
import { loginRequired } from './loginRequired';

test('authorization으로 넘어온 토큰이 정상일 경우 다음 미들웨어로 간다.', () => {
  const req = {
    headers: {
      authorization: 'bearerToken thisIsBearerToken',
    },
  };
  const res = jest.fn();
  const next = jest.fn();

  jwt.verify.mockReturnValue({
    userId: 'success user',
  });

  loginRequired(req, res, next);

  expect(next).toBeCalledWith();
  expect(next).not.toBeCalledWith(
    new UnauthorizedError('로그인한 유저만 사용할 수 있는 서비스입니다.')
  );
  expect(next).not.toBeCalledWith(
    new UnauthorizedError('정상적인 토큰이 아닙니다.')
  );
});
```

👉 &nbsp; jsonwebtoken 모듈을 사용하는데 이를 Mock 함수로 만들었다. 그리고 jwt의 verify 메서드는 {userId: 'success user'}를 return하게 만들어서 무조건 error가 나지 않도록 했다.

&nbsp; 따라서 next함수가 매개변수 없이 실행되고, 이를 검증하는 코드를 3개 작성했다.

<br>

# 📌 &nbsp; 느낀점

👉 &nbsp; 아직 Jest에 대해 잘 알지 못하고, 배워야할게 많지만 적용을 해보니 너무 재미있었다. 다음 프로젝트에서는 테스트 코드를 먼저 짜고 나중에 코드를 짜보는 방식으로 하면 좋겠다는 생각이 들었다.

또한 CRUD에 대한 검증과 DB연동 관련한 부분은 추후 업로드 하도록 해야겠다!!
