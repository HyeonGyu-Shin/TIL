# ๐ย What is Middleware

๐ย  ๊ณต์๋ฌธ์์ ๋ฐ๋ฅด๋ฉด ๋ฏธ๋ค์จ์ด๋ ํจ์๋ค. ์ด๋ค ํจ์๋๋ฉด ์์ฒญ์์ ์๋ต๊น์ง์ ์ฃผ๊ธฐ์์ ์์ฒญ ๊ฐ์ฒด(req)์ ์๋ต ๊ฐ์ฒด(res) ๊ทธ๋ฆฌ๊ณ  ๋ค์ ๋ฏธ๋ค์จ์ด ๊ธฐ๋ฅ์ ์ ๊ทผํ  ์ ์๋ ๊ธฐ๋ฅ์ ์ง๋ ํจ์์ด๋ค.

๋ฏธ๋ค์จ์ด๋ ๋ค์์ ์ผ๋ค์ ์ํํ  ์ ์๋ค.

- ๋ชจ๋  ์ฝ๋์ ์คํ
- ์์ฒญ ๊ฐ์ฒด์ ์๋ต ๊ฐ์ฒด ๋ณ๊ฒฝ
- ์์ฒญ~์๋ต์ ์ฃผ๊ธฐ ๋๋ด๊ธฐ
- ์คํ์์ ๋ค์ ๋ค์ ๋ฏธ๋ค์จ์ด ํธ์ถ

๋ง์ฝ ํ์ฌ ๋ฏธ๋ค์จ์ด๊ฐ ์์ฒญ~์๋ต ์ฃผ๊ธฐ๋ฅผ ๋๋ด์ง ์๋๋ค๋ฉด ๋ฐ๋์ `next()` ๋ฅผ ์ด์ฉํ์ฌ ๋ค์ ๋ฏธ๋ค์จ์ด์ ์ ์ด๋ฅผ ๋๊ฒจ์ค์ผ ํ๋ค. ๊ทธ๋ ์ง ์์ผ๋ฉด ์์ฒญ์ ์ค๋จ๋๋ค.

# ๐ย Types of Middleware

๐ย  Express์์๋ ๋ค์ ์ ํ์ ๋ฏธ๋ค์จ์ด๋ค์ ์ฌ์ฉํ  ์ ์๋ค.

- Application-level middleware
- Router-level middleware
- Error-handling middleware
- Built-in middleware
- Third-party middleware

์ ๊ทธ๋ผ ํ๋์ฉ ์ดํด๋ณด๋๋ก ํ์!

## **๐ย Application-level middleware**

๐ย  app.use()์ app.METHOD() ํจ์๋ฅผ ์ฌ์ฉํ์ฌ express()๋ก ์์ฑํ app ์ธ์คํด์ค์ ๋ฏธ๋ค์จ์ด๋ฅผ ๋ฐ์ธ๋ฉํ๋ ๋ฏธ๋ค์จ์ด์ด๋ค.

path๋ฅผ ๊ฐ์ง๊ณ  ์์ง ์์ ๋ฏธ๋ค์จ์ด๋ ์์ฒญ์ด ๋ค์ด์ฌ ๋๋ง๋ค ์คํ๋๋ค.

```jsx
const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log('Time: ', Date.now());
  next();
});
```

next(โrouteโ)๋ฅผ ์ฌ์ฉํ๋ฉด ํ์ฌ stack์์ ๋ฒ์ด๋ ๋ฐ์ ์๋ Application-level ๋ฏธ๋ค์จ์ด๋ก ์ ์ด๋ฅผ ๋๊ธด๋ค.

```jsx
app.get(
  '/user/:id',
  (req, res, next) => {
    // id๊ฐ์ด 0์ด๋ฉด ๋ฐ์ ์๋ Application-level ๋ฏธ๋ค์จ์ด๋ก ์ ์ด๋ฅผ ๋๊ธด๋ค.
    if (req.params.id === '0') next('route');
    // id๊ฐ์ด 0์ด ์๋๋ฉด ํ์ฌ์ stack์ ์๋ ๋ค์ ๋ฏธ๋ค์จ์ด๋ก ์ ์ด๋ฅผ ๋๊ธด๋ค.
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

๋ฏธ๋ค์จ์ด๋ ์ฌ์ฌ์ฉ์ฑ์ ์ํด ๋ฐฐ์ด๋ก ์ ์ธ๋  ์ ์๋ค.

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

## **๐ย Router-level middleware**

๐ย Router-level์ ๋ฏธ๋ค์จ์ด๋ express.Router()์ ์ธ์คํด์ค์ ๋ฐ์ธ๋ฉ ๋๋ค๋ ์ ์ ์ ์ธํ๋ฉด Application-level ๋ฏธ๋ค์จ์ด์ ๋์ผํ ๋ฐฉ์์ผ๋ก ์๋ํ๋ค.

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

express.Router()์ ์ธ์คํด์ค์ธ router์ ๋ฐ์ธ๋ฉ๋๋ค๋ ์ ๋ง ๋นผ๋ฉด Application-level middleware์ ๋๊ฐ์ด ๋์ํ๋ ๋ชจ์ต์ ๋ณผ ์ ์๋ค.

## **๐ย Error-handling middleware**

๐ย  Error-handling middleware๋ ๋ฌด์กฐ๊ฑด 4๊ฐ์ ์ธ์(err, req, res, next)๋ฅผ ๋ฐ์์ผ ํ๋ค.
๊ทธ๋ ์ง ์์ผ๋ฉด ์ผ๋ฐ ๋ฏธ๋ค์จ์ด๋ก ์ธ์ํ๋ค.

```jsx
app.use((err, req, res, next) => {
	console.error(err.stack);
	res.status(500).send('Something broke!);
})
```

์์ธํ ์ค๋ฅ์ฒ๋ฆฌ ๋ถ๋ถ์ ์ถํ ์์ฑ

## **๐ย Built-in middleware**

๐ย  express์ ๊ธฐ๋ณธ์ ์ผ๋ก ์ ์๋์ด ์๋ ๋ฏธ๋ค์จ์ด์ด๋ค. ํ์ํ๋ฉด ์ด ๋ฏธ๋ค์จ์ด๋ฅผ ์ฌ์ฉํ  ์ ์๋ค.

- express.static - HTML, image ๋ฑ๋ฑ์ ์ ์  ์์ฐ์ ์ ๊ณตํด์ค๋ค.
- express.json - JSON payload๋ก ๋ค์ด์ค๋ ์์ฒญ์ ๊ตฌ๋ฌธ ๋ถ์ํด์ค๋ค.
- express.urlencoded - URL ์ธ์ฝ๋ฉ ํ์ด๋ก๋๋ก ๋ค์ด์ค๋ ์์ฒญ์ ๊ตฌ๋ฌธ ๋ถ์ํด์ค๋ค.

## **๐ย Third-party middleware**

๐ย  third-party middleware๋ฅผ ์ฌ์ฉํ์ฌ Express app์ ๊ธฐ๋ฅ์ ๋ํ  ์ ์๋ค.
