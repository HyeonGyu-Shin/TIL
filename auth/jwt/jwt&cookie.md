<br>

# JWT와 Cookie를 함께 이용하기

<br>

<br>

## Cookie와 Session??

<br>

👉 &nbsp; 쿠키와 세션은 다음과 같은 차이가 있다.

1. Session - 클라이언트의 정보를 서버 측 저장소에 저장하고 사용한다.

2. Cookie - 클라이언트의 정보를 클라이언트(브라우저)에 저장하고 사용한다.

<br>

## 내가 기존에 사용하던 방식

<br>

👉 &nbsp; 기존에는 Session을 이용하여 로그인을 관리했다.

1. Cookie에 Session ID를 저장한다.

2. 클라이언트에서 요청이 들어오면 Cookie에 있는 Session ID를 확인한다.

3. 받아온 Session ID를 이용하여 Session Store에서 유저 정보를 가져왔다.

4. 유저 정보와 일치하다면 로그인 성공

<br>

## JWT + Cookie 방식은??

<br>

👉 &nbsp; JWT와 Cookie를 이용한 방식은 다음과 같다.

1. Cookie에 JWT를 저장한다.

2. 클라이언트에서 요청이 들어오면 Cookie에 있는 JWT를 확인한다.

3. 받아온 JWT를 서명 확인한 후, 유저 정보가 일치한다면 로그인 성공!

&nbsp; DB 접근이 줄어서 효율적인 인증 구현이 가능하다.

<br>

## 코드를 통해 알아보도록 하자.

<br>

👉 &nbsp; 로그인 로직에 JWT 토큰을 생성하고 쿠키를 전달하는 코드이다.

```js
const setUserToken = (res, user) => {
    const token = jwt.sign(user, secret);
    // jwt 토큰 생성

    res.cookie('token', token);
    // 쿠키에 넣어준다.
};

---

router.post('/', passport.authenticate('local'), (req, res,next) => {
    setUserToken(res, req.user);
    // 인증 성공시에 jwt 토큰 발행

    res.redirect('/');
});

```

👉 &nbsp; jwt 토큰을 생성하고, 쿠키에 넣어주는 함수를 만들었다.

&nbsp; 그 후, 로그인에 성공하면 위에서 정의한 함수를 실행해준다. 그 결과 토큰이 브라우저의 Cookie로 저장이 된다.

&nbsp; 다음은 passport-jwt를 사용하는 방법이다.

```js
const jwtStrategy = require('passport-jwt').Strategy;

const cookieExtractor = (req) => {
    const { token } = req.cookies;

    return token;
};

const opts = {
    secretOrKeyt: secret,
    jwtFromRequest: cookieExtractor,
};

module.exports = new JwtStrategy(opts, (user, done) => {
    done(null, user);
});

---

passport.use(jwt);
```

👉 &nbsp; 다음은 JWT 미들웨어를 추가하는 코드이다.

```js
app.use((req, res, next) => {
    if (!req.cookies.token) {
        next();
        return;
    }

    return passport.authenticate('jwt')(req, res, next);
});
```

👉 &nbsp; 다음은 로그아웃을 하는 코드이다.

```js

res.cookie('token', null, {
    maxAge: 0;
})

```

👉 &nbsp; cookie의 만료 시간을 0으로 설정하여 클라이언트가 쿠키를 바로 만료시키도록 한다.

<br>

# 정리

<br>

👉 &nbsp; 개념에 대해서는 이해를 했지만, 아직 완전하게 코드로 구현하기엔 힘든 것 같다. 추후에 직접 코드를 작성해가면서 내 것으로 만들 수 있도록 해야겟다.
