<br>

# ๐ &nbsp; Elice 1์ฐจ ํ๋ก์ ํธ์ Jest๋ฅผ ์ ์ฉํด๋ณด์!

<br>

๐ &nbsp; Elice 1์ฐจ ํ๋ก์ ํธ๋ฅผ ํ๋ฉด์ ๋ง์ ๊ฒ๋ค์ ๋๋ผ๊ณ  ์๋ค.

&nbsp; ํ๋ก์ ํธ๊ฐ ๊ณ์ ์งํ๋๋ฉด์ ์คํค๋ง๊ฐ ์๊พธ ๋ณ๊ฒฝ๋๋ค ๋ณด๋ ๊ธฐ์กด์ ๋ง๋ค์ด๋๋ ์๋น์ค๋ ์ปจํธ๋กค๋ฌ์ ๋ํ ์ ๋ขฐ์ฑ์ด ์๊พธ ๋จ์ด์ง๋ ๋ฌธ์ ๊ฐ ๋ฐ์ํ๋ค.

&nbsp; ์ง๊ธ๊น์ง ์์์์ผ๋ก Postman์ ํตํด API๋ค์ ์ฒดํฌํด์์๋ค. ๋ง์ฐํ๊ฒ ํ์คํธ ์ฝ๋๋ ์ง๋ด์ผ์ง๋ผ๋ ์๊ฐ์ ๊ฐ์ง๊ณ  ์์๋๋ฐ, Jest๋ฅผ ๋์ํ๋ฉด ์ข๊ฒ ๋ค๋ feedback์ ๋ฐ๊ฒ ๋์๋ค.

&nbsp; ๋ฐ๋ผ์ ์ด๋ฒ ๊ธฐํ์ Jest์ ๋ํด ๋ฐฐ์๋ณด๊ณ  ํ๋ก์ ํธ์ ์ ์ฉํด๋ณด๋ ค๊ณ  ํ๋ค.

<br>

# ๐ &nbsp; Jest์ ๋ํด ๋ฐฐ์๋ณด์!!

<br>

<br>

## ๐ &nbsp; What is Jest...?

<br>

๐ &nbsp; Jest๋ ๋จ์์ฑ์ ์ค์ ์ ๋ JS์ ํ์คํ ํ๋ ์์ํฌ๋ผ๊ณ  ๊ณต์๋ฌธ์์ ๋์์๋ค. ๋ฐ๋ฒจ, TS, Node, React ๋ฑ๋ฑ์์ ์ฌ์ฉํ  ์ ์๋ค๊ณ  ํ๋ค.

&nbsp; ํ์คํธ๋ฅผ ์ฝ๊ฒ ํ๊ธฐ ์ํด ๋ง๋ค์ด์ง ํ์คํธ ํ๋ ์์ํฌ๋ผ๋ ๊ฑธ ์๊ฒ๋์๋ค.

<br>

# ๐ &nbsp; Jest๋ฅผ ํ๋ก์ ํธ์ ์ ์ฉํด๋ณด์!

<br>

๐ &nbsp; CRUD์ ๋จผ์  ์ ์ฉ์ ํ๊ณ ์ถ์์ผ๋... DB์์ ์ฐ๊ด์ฑ ๋๋ฌธ์ ์ํ์ฐฉ์ค๋ฅผ ๋ง์ด ๊ฒช์๋ค...ใ ใ  ๋ฐ๋ผ์ ๋ก๊ทธ์ธ์ ๊ฒ์ฌํ๋ ๋ฏธ๋ค์จ์ด์ ๋จผ์  ์ ์ฉํด๋ณด๋ ค๊ณ  ํ๋ค.

```js
// loginRequired.js

import jwt from 'jsonwebtoken';
import { UnauthorizedError } from '../utils';

function loginRequired(req, res, next) {
  const userToken = req.headers['authorization']?.split(' ')[1];

  if (!userToken) {
    return next(
      new UnauthorizedError('๋ก๊ทธ์ธํ ์ ์ ๋ง ์ฌ์ฉํ  ์ ์๋ ์๋น์ค์๋๋ค.')
    );
  }

  try {
    const secretKey = process.env.JWT_SECRET_KEY || 'secret-key';
    const jwtDecoded = jwt.verify(userToken, secretKey);
    req.currentUserId = jwtDecoded.userId;

    next();
  } catch (error) {
    return next(new UnauthorizedError('์ ์์ ์ธ ํ ํฐ์ด ์๋๋๋ค.'));
  }
}

export { loginRequired };
```

๐ &nbsp; ํ์คํธ๋ฅผ ์งค ๋๋ ๋ถ๊ธฐ๋ฅผ ๊ธฐ์ค์ผ๋ก ๋๋๋ผ๊ณ  ๋ค์๋ค. ์์ ๋ฏธ๋ค์จ์ด์ ๊ฒฝ์ฐ 3๊ฐ์ง์ ๋ถ๊ธฐ๋ก ๋๋ ์ ์๋ค.

1. userToken์ด ์กด์ฌํ์ง ์๋ ๊ฒฝ์ฐ

2. userToken์ด ์กด์ฌํ์ง๋ง ์ ์์ ์ธ ํ ํฐ์ด ์๋ ๊ฒฝ์ฐ

3. userToken์ด ์กด์ฌํ๊ณ  ์ ์์ ์ธ ํ ํฐ์ผ ๊ฒฝ์ฐ

&nbsp; ์ ๊ทธ๋ผ ์ด์  ๊ฐ ๋ถ๊ธฐ๋ณ๋ก ํ์คํธ ์ฝ๋๋ฅผ ๋ง๋ค๋ฌ ๊ฐ๋ณด์!

<br>

### โ๏ธ &nbsp; 1. userToken์ด ์กด์ฌํ์ง ์๋ ๊ฒฝ์ฐ

<br>

```js
// loginRequired.test.js

import { UnauthorizedError } from '../utils';
import { loginRequired } from './loginRequired';

test('req.headers.authorization์ด ์์ด์ ์๋ฌ๋ฅผ ๋์ง๋ค.', () => {
  const req = {
    headers: {},
  };
  const res = jest.fn();
  const next = jest.fn();

  loginRequired(req, res, next);

  expect(next).toBeCalledWith(
    new UnauthorizedError('๋ก๊ทธ์ธํ ์ ์ ๋ง ์ฌ์ฉํ  ์ ์๋ ์๋น์ค์๋๋ค.')
  );
});
```

๐ &nbsp; req.headers์ authorization ๊ฐ์ด ์๋ค. ๋ฐ๋ผ์ userToken์ด undefined์ด๊ธฐ ๋๋ฌธ์ ์กฐ๊ฑด๋ฌธ์ ๊ฑธ๋ ค์ next(error ๊ฐ์ฒด)๋ฅผ ์คํํ๊ณ  ๋ฏธ๋ค์จ์ด๋ฅผ ์ข๋ฃํ๋ค.

&nbsp; ์ด๋ฅผ ๊ฒ์ฆํ๊ธฐ ์ํด next ํจ์๊ฐ `new UnauthorizedError('๋ก๊ทธ์ธํ ์ ์ ๋ง ์ฌ์ฉํ  ์ ์๋ ์๋น์ค์๋๋ค.')`๋ฅผ ์ธ์๋ก ํธ์ถ๋์๋์ง ํ์ธํ๋ ์ฝ๋๋ฅผ ์์ฑํ๋ค.

<br>

### โ๏ธ &nbsp; 2. userToken์ด ์กด์ฌํ์ง๋ง ์ ์์ ์ธ ํ ํฐ์ด ์๋ ๊ฒฝ์ฐ

<br>

```js
// loginRequired.test.js

import { UnauthorizedError } from '../utils';
import { loginRequired } from './loginRequired';

test('req.headers.authorization์ ์์ง๋ง ์ ์์ ์ธ ํ ํฐ์ด ์๋๋ค.', () => {
  const req = {
    headers: {
      authorization: 'bearerToken thisIsBearerToken',
    },
  };
  const res = jest.fn();
  const next = jest.fn();

  loginRequired(req, res, next);

  expect(next).toBeCalledWith(
    new UnauthorizedError('์ ์์ ์ธ ํ ํฐ์ด ์๋๋๋ค.')
  );
});
```

๐ &nbsp; req.headers.authorization์ ์ฌ๋ฐ๋ฅธ ํ ํฐ ๊ฐ์ด ๋ค์ด์๊ธฐ ๋๋ฌธ์ userToken์ ๊ฐ์ด ์กด์ฌํ๊ฒ ๋๊ณ  if๋ฌธ์ ํต๊ณผํ๋ค. ํ์ง๋ง try๋ฌธ์์ ์๋ฌ๊ฐ ๋ฐ์ํ๊ธฐ ๋๋ฌธ์ next(error ๊ฐ์ฒด)๋ฅผ ์คํํ๊ณ  ๋ฏธ๋ค์จ์ด๊ฐ ์ข๋ฃ๋๋ค.

&nbsp; ์ด๋ฅผ ๊ฒ์ฆํ๊ธฐ ์ํด next ํจ์๊ฐ ` new UnauthorizedError('์ ์์ ์ธ ํ ํฐ์ด ์๋๋๋ค.')`๋ฅผ ์ธ์๋ก ํธ์ถ๋์๋์ง ํ์ธํ๋ ์ฝ๋๋ฅผ ์์ฑํ๋ค.

<br>

### โ๏ธ &nbsp; 3. userToken์ด ์กด์ฌํ๊ณ  ์ ์์ ์ธ ํ ํฐ์ผ ๊ฒฝ์ฐ

<br>

```js
jest.mock('jsonwebtoken');
import jwt from 'jsonwebtoken';
import { UnauthorizedError } from '../utils';
import { loginRequired } from './loginRequired';

test('authorization์ผ๋ก ๋์ด์จ ํ ํฐ์ด ์ ์์ผ ๊ฒฝ์ฐ ๋ค์ ๋ฏธ๋ค์จ์ด๋ก ๊ฐ๋ค.', () => {
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
    new UnauthorizedError('๋ก๊ทธ์ธํ ์ ์ ๋ง ์ฌ์ฉํ  ์ ์๋ ์๋น์ค์๋๋ค.')
  );
  expect(next).not.toBeCalledWith(
    new UnauthorizedError('์ ์์ ์ธ ํ ํฐ์ด ์๋๋๋ค.')
  );
});
```

๐ &nbsp; jsonwebtoken ๋ชจ๋์ ์ฌ์ฉํ๋๋ฐ ์ด๋ฅผ Mock ํจ์๋ก ๋ง๋ค์๋ค. ๊ทธ๋ฆฌ๊ณ  jwt์ verify ๋ฉ์๋๋ {userId: 'success user'}๋ฅผ returnํ๊ฒ ๋ง๋ค์ด์ ๋ฌด์กฐ๊ฑด error๊ฐ ๋์ง ์๋๋ก ํ๋ค.

&nbsp; ๋ฐ๋ผ์ nextํจ์๊ฐ ๋งค๊ฐ๋ณ์ ์์ด ์คํ๋๊ณ , ์ด๋ฅผ ๊ฒ์ฆํ๋ ์ฝ๋๋ฅผ 3๊ฐ ์์ฑํ๋ค.

<br>

# ๐ &nbsp; ๋๋์ 

๐ &nbsp; ์์ง Jest์ ๋ํด ์ ์์ง ๋ชปํ๊ณ , ๋ฐฐ์์ผํ ๊ฒ ๋ง์ง๋ง ์ ์ฉ์ ํด๋ณด๋ ๋๋ฌด ์ฌ๋ฏธ์์๋ค. ๋ค์ ํ๋ก์ ํธ์์๋ ํ์คํธ ์ฝ๋๋ฅผ ๋จผ์  ์ง๊ณ  ๋์ค์ ์ฝ๋๋ฅผ ์ง๋ณด๋ ๋ฐฉ์์ผ๋ก ํ๋ฉด ์ข๊ฒ ๋ค๋ ์๊ฐ์ด ๋ค์๋ค.

๋ํ CRUD์ ๋ํ ๊ฒ์ฆ๊ณผ DB์ฐ๋ ๊ด๋ จํ ๋ถ๋ถ์ ์ถํ ์๋ก๋ ํ๋๋ก ํด์ผ๊ฒ ๋ค!!
