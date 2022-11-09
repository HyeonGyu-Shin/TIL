# 📌 What is Middleware

👉  공식문서에 따르면 미들웨어는 함수다. 어떤 함수냐면 요청에서 응답까지의 주기에서 요청 객체(req)와 응답 객체(res) 그리고 다음 미들웨어 기능에 접근할 수 있는 기능을 지닌 함수이다.

미들웨어는 다음의 일들을 수행할 수 있다.

- 모든 코드의 실행
- 요청 객체와 응답 객체 변경
- 요청~응답의 주기 끝내기
- 스택에서 다음 다음 미들웨어 호출

만약 현재 미들웨어가 요청~응답 주기를 끝내지 않는다면 반드시 `next()` 를 이용하여 다음 미들웨어에 제어를 넘겨줘야 한다. 그렇지 않으면 요청은 중단된다.

# 📌 Types of Middleware

👉  Express에서는 다음 유형의 미들웨어들을 사용할 수 있다.

- Application-level middleware
- Router-level middleware
- Error-handling middleware
- Built-in middleware
- Third-party middleware

자 그럼 하나씩 살펴보도록 하자!

## **🔎 Application-level middleware**

👉  app.use()와 app.METHOD() 함수를 사용하여 express()로 생성한 app 인스턴스에 미들웨어를 바인딩하는 미들웨어이다.

path를 가지고 있지 않은 미들웨어는 요청이 들어올 때마다 실행된다.

```jsx
const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log('Time: ', Date.now());
  next();
});
```

next(’route’)를 사용하면 현재 stack에서 벗어나 밑에 있는 Application-level 미들웨어로 제어를 넘긴다.

```jsx
app.get(
  '/user/:id',
  (req, res, next) => {
    // id값이 0이면 밑에 있는 Application-level 미들웨어로 제어를 넘긴다.
    if (req.params.id === '0') next('route');
    // id값이 0이 아니면 현재의 stack에 있는 다음 미들웨어로 제어를 넘긴다.
    else next();
  },
  (req, res, next) => {
    // send a regular response
    res.send('regular');
  }
);

// handler for the /user/:id path, which sends a special response
app.get('/user/:id', (req, res, next) => {
  res.send('special');
});
```

미들웨어는 재사용성을 위해 배열로 선언될 수 있다.

```jsx
function logOriginalUrl(req, res, next) {
  console.log('Request URL:', req.originalUrl);
  next();
}

function logMethod(req, res, next) {
  console.log('Request Type:', req.method);
  next();
}

const logStuff = [logOriginalUrl, logMethod];
app.get('/user/:id', logStuff, (req, res, next) => {
  res.send('User Info');
});
```

## **🔎 Router-level middleware**

👉 Router-level의 미들웨어는 express.Router()의 인스턴스에 바인딩 된다는 점을 제외하면 Application-level 미들웨어와 동일한 방식으로 작동한다.

```jsx
const express = require('express');
const app = express();
const router = express.Router();

// a middleware function with no mount path. This code is executed for every request to the router
router.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
});

// a middleware sub-stack shows request info for any type of HTTP request to the /user/:id path
router.use(
  '/user/:id',
  (req, res, next) => {
    console.log('Request URL:', req.originalUrl);
    next();
  },
  (req, res, next) => {
    console.log('Request Type:', req.method);
    next();
  }
);

// a middleware sub-stack that handles GET requests to the /user/:id path
router.get(
  '/user/:id',
  (req, res, next) => {
    // if the user ID is 0, skip to the next router
    if (req.params.id === '0') next('route');
    // otherwise pass control to the next middleware function in this stack
    else next();
  },
  (req, res, next) => {
    // render a regular page
    res.render('regular');
  }
);

// handler for the /user/:id path, which renders a special page
router.get('/user/:id', (req, res, next) => {
  console.log(req.params.id);
  res.render('special');
});

// mount the router on the app
app.use('/', router);
```

express.Router()의 인스턴스인 router에 바인딩된다는 점만 빼면 Application-level middleware와 똑같이 동작하는 모습을 볼 수 있다.

## **🔎 Error-handling middleware**

👉  Error-handling middleware는 무조건 4개의 인수(err, req, res, next)를 받아야 한다.
그렇지 않으면 일반 미들웨어로 인식한다.

```jsx
app.use((err, req, res, next) => {
	console.error(err.stack);
	res.status(500).send('Something broke!);
})
```

자세한 오류처리 부분은 추후 작성

## **🔎 Built-in middleware**

👉  express에 기본적으로 정의되어 있는 미들웨어이다. 필요하면 이 미들웨어를 사용할 수 있다.

- express.static - HTML, image 등등의 정적 자산을 제공해준다.
- express.json - JSON payload로 들어오는 요청을 구문 분석해준다.
- express.urlencoded - URL 인코딩 페이로드로 들어오는 요청을 구문 분석해준다.

## **🔎 Third-party middleware**

👉  third-party middleware를 사용하여 Express app에 기능을 더할 수 있다.
