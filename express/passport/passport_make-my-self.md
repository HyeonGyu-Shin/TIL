<br>

# 직접 만들어보면서 이해하는 passport!

<br>

<br>

# passport에 대해 알아봅시다!

<br>

👉 &nbsp; passport는 로그인 처리를 쉽게 할 수 있도록 도와주는 라이브러리이다.

<br>

## 준비

<br>

👉 &nbsp; 웹 어플리케이션에서 인증을 구현하기 위한 방법으로 세션과 쿠키를 주로 사용한다. 흐름은 다음과 같다.

1. 접속한 브라우저의 정보를 서버의 세션DB에 저장, 세션 아이디를 브라우저에 쿠키로 전달한다.

2. 브라우저는 쿠키에 담긴 세션 아이디를 저장, 다음 요청부터는 헤더에 세션 아이디를 담아 서버로 전송한다.

3. 서버는 넘어온 세션 값을 가지고 이전에 접속한 브라우저임을 식별할 수 있다.

<br>

### 세션 설치

<br>

👉 &nbsp; passport에서 session을 이용하기 위해 express-session 미들웨어를 가져온다.

```js
// app.js

const session = require('express-session');

app.use(
    session({
        name: 'mySession', // 쿠키에 저장될 세션키 이름
        secret: '1q2w3e4r', // 세션 암호화를 위한 시크릿
        resave: true,
        saveUninitialize: true,
    })
);
```

👉 &nbsp; 세션 데이터를 브라우저에서 확인하기 위해 GET /debug 엔드포인트를 하나 만든다.

```js
app.get('/debug', (req, res) => {
    res.json({
        'req.session': req.session,
        'req.user': req.user,
        'req._passport': req._passport,
    });
});
```

👉 &nbsp; 브라우저로 '/debug' url로 접속하면 세션 정보를 확인할 수 있다.

```js
    {
  "req.session": {
    "cookie": {
      "originalMaxAge": null,
      "expires": null,
      "httpOnly": true,
      "path": "/"
    }
  }
}

```

👉 &nbsp; 위에 보이는 cookie 객체가 요청한 클라이언트로 전송될 것이다.

<br>

### 로그인 폼 & 로그인 폼 처리

<br>

👉 &nbsp; 로그인 폼 & 로그인 폼 처리에 관한 건 이 글에서는 다루지 않도록 하겠다. 다만 흐름이 어떻게 되는지에 대해서만 간략하게 알아보자.

1. username과 password를 받는 폼이 작성된 html 페이지를 클라이언트에 제공해준다.

2. 제공된 폼을 클라이언트가 입력하면 '/login' url로 POST 요청을 하게 된다. 이때, req.body에 username과 password가 저장되어 전송된다.

3. 서버에서 POST '/login'에 대한 처리를 해준다. 이 곳에서부터 인증이 들어가야 한다.

<br>

## 직접 만들어보자 MyPassport!

<br>

👉 &nbsp; 원글의 의도처럼 나도 passport를 비슷하게 만들어보면서 동작 원리를 이해하려고 한다.

<br>

### 로그인 기능 - authenticate()

<br>

👉 &nbsp; 간단하게 구현하려고 했기 때문에 실제 DB 대신 유저 객체가 담긴 배열을 이용하려고 한다.

```js
// ./db/index.js

exports.Users = Users = [
    {
        username: '1',
        password: '1',
        id: 1,
    },
];

exports.identify = (username, password) => {
    const user = Users.find((u) => {
        if (username !== u.username) {
            return undefined;
        }

        if (password !== u.password) {
            return undefined;
        }

        return u;
    });

    return user;
};
```

&nbsp; `identify` 함수는 DB를 돌면서 req.body로 넘어온 username과 password가 일치한 유저 객체를 찾아 반환해준다. 만약 없다면 undefined를 반환해준다.

<br>

👉 &nbsp; 인증 기능을 수행하는 MyPassport 클래스를 만든 후, `authenticate` 메서드를 추가해보자.

```js
// MyPassport.js

const { identify } = require('./db');

class MyPassport {
    #key = 'userId';

    authenticate() {
        return (req, res, next) => {
            try {
                const { username, password } = req.body;
                // 클라이언트로부터 넘어온 username과 password 정보

                const user = identify(username, password);
                // identify 함수는 위에서 정의한 DB에서 회원 객체를 가져오는 함수이다.

                if (!user) return res.sendStatus(401);

                req.session[this.#key] = user.id;
            } catch (err) {
                next(err);
            }
        };
    }
}

module.exports = new MyPassport();
// 싱글톤
```

👉 &nbsp; 세션에 로그인 정보를 저장할 때 사용할 속성 이름으로 #key라는 클래스 멤버 변수를 만들었다.

&nbsp; `authenticate()` 메서드는 실제로 passport에서 로그인을 수행하는 역할을 한다고 한다. express와의 호환을 위해서 미들웨어 함수를 반환했다.

&nbsp; 유저를 찾으면 세션에 인증 정보를 저장한다. 유저 아이디만 저장해서 최소한의 데이터만 세션에 유지하도록 했다.

<br>

### 로그인 컨트롤러

<br>

👉 &nbsp; 이제 위에서 만든 MyPassport 모듈을 이용하여 로그인 엔드포인트에서 로그인 처리를 해보자.

```js
app.post('/login', mypassport.authenticate(), (req, res) =>
    res.send('로그인 성공');
);
```

&nbsp; `authenticate()` 메서드가 반환한 미들웨어를 컨트롤러 함수 직전에 전달해서 인증과정을 먼저 수행하도록 했다.

&nbsp;덕분에 인증에 성공해야 컨트롤러 로직이 수행되는 것을 보장할 수 있다.

&nbsp; 로그인 후 세션 상태를 보면 userId key의 value로 사용자의 아이디가 있다.

<br>

### 세션에서 로그인 상태 복구 - session()

<br>

> express는 요청 정보를 모두 Request 객체가 담고 있다. 유저 정보도 마찬가지로 `req.user`에 할당해서 관리하면 편할 것 같다. 컨트롤러 로직에서는 Request 객체를 통해 유저 정보에 접근하는 것이 보다 일관적인 인터페이스이기 때문이다.

👉 &nbsp; 원글의 작성자분께서 작성해주신 내용이다.
req 객체에 프로퍼티를 추가하여 사용하는게 더 편하고 일관된 인터페이스라고 한다.😄

&nbsp; 자 그럼 모든 요청의 앞단에서 세션에 있는 로그인 정보를 req.user에 넣어주는 코드를 작성해보자.

```js
class MyPassport {
    session() {
        return (req, res, next) => {
            try {
                // 세션에 로그인 정보가 있다면
                if (req.session[this.#key]) {
                    const user = Users.find(
                        (u) => String(u.id) === req.session[this.#key]
                    );
                    // 세션의 로그인 정보를 통해 유저 객체를 DB에서 가져온다.

                    if (user) req.user = user;
                    // user가 존재한다면 req.user에 user 객체를 넣어준다.
                }
                next();
            } catch (err) {
                next(err);
            }
        };
    }
}
```

&nbsp; `session` 메서드는 세션에 저장된 로그인 정보를 이용해 유저 객체를 찾고, 그 후 이를 이용하여 DB에서 유저 객체를 찾고 req.user에 할당해주는 메서드이다.

&nbsp; 이제 이 메서드를 어플리케이션에 적용하자.

```js
// app.js

app.use(
    session({
        ...
    })
);

app.use(myPassport.session())

```

👉 &nbsp; 세션 활성화 직 후 세션에 저장된 로그인 정보를 복구하도록 순서에 맞게 미들웨어를 세팅했다.

&nbsp; '/debug' url로 접근하면 req.user에 유저 정보가 할당된 모습을 확인할 수 있다.
