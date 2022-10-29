<br>

# Session에 대해서 알고 갑시다!

<br>

<br>

## Session이란??

<br>

👉 &nbsp; 웹 서버가 클라이언트의 정보를 클라이언트별로 구분하여 서버에 저장하고

&nbsp; 클라이언트 요청 시 session id를 사용하여 클라이언트의 정보를 다시 확인하는 기술이다.

<br>

## Session의 작동 방식

<br>

👉 &nbsp; 서버는 세션을 생성하여 세션의 구분자인 session id를 클라이언트에 전달한다.

&nbsp; 클라이언트는 요청 시 session id를 함께 요청에 담아서 전송한다.

&nbsp; 서버는 전달받은 session id로 해당하는 세션을 찾아 클라이언트 정보를 확인한다.

<br>

## Express.js의 Session

<br>

👉 &nbsp; express-session 패키지를 사용하여 간단하게 session 동작을 구현할 수 있다.

&nbsp; 특별한 설정 없이 자동으로 session 동작을 구현해준다. 자동으로 session id를 클라이언트에 전달해주고, session id로 클라이언트의 정보를 확인한다.

<br>

# Session-store에 대해 배워봅시다.

<br>

<br>

## Session-store를 사용하는 이유

<br>

👉 &nbsp; Session이 무엇이고, 어떻게 동작하는지에 대해 알게 되었다. 그런데 Session Store를 써야할 필요성이 느껴졌나???

&nbsp; 느껴진다! express-session 패키지는 session을 기본적으로 메모리에 저장한다.

&nbsp; 따라서 현재의 server가 종료된다면 모든 유저의 로그인이 해제된다.또한, 서버가 여러 대가 있을 경우, 서버 간 세션 정보를 공유할 수 없다.

<br>

<img src="./session-store.png" alt="session store를 설명하는 사진">

<br>

<br>

## MongoDB를 이용하여 Session Store를 구현해보자!

<br>

👉 &nbsp; connect-mongo 패키지를 이용하여 MongoDB를 session store로 사용할 수 있다.

&nbsp; connect-mongo 패키지는 express-session 패키지의 옵션으로 전달 가능하다. 또한 자동으로 session 값이 변경될 때 update되고, session이 호출될 때 find 해주는 장점이 있다.

&nbsp; 코드를 통해 살펴보자.

```js
const MongoStore = require('connect-mongo');

app.use(
    session({
        secret: 'SeCrEt',
        resave: false,
        saveUninitialized: true,
        store: MongoStore.create({
            mongoUrl: 'mongoUrl',
        }),
    })
);
```

&nbsp; express-session 설정 시 store 옵션에 전달하고, MongoUrl을 설정해주면 된다.

<br>

## 한 번 해보자!!

<br>

👉 &nbsp; 코드를 작성 후 실행을 했다. 한 번 DB를 살펴보자.

<br>

<img src="./connect-mongo.png">

<br>

&nbsp; sessions collection이 생겼고, 그 안에 session이 성공적으로 들어와있는 모습을 볼 수 있다.

&nbsp; 서버를 종료한 후, 다시 시작하고 req.user가 존재해야만 접근할 수 있는 '/profile'에 접근한 결과 성공적으로 접근한 모습을 볼 수 있었다.

<br>

# 정리

<br>

👉 &nbsp; 아직 세션에 대해 잘 안다고 말할 수는 없지만, 어느 정도의 이해를 한 것 같다. 이를 이용하여 다양한 로직을 짤 수 있도록 더욱 더 공부해야겠다!
