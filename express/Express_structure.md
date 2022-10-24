<br>

# Express 구조를 살펴봅시다!

<br>

👉 &nbsp; Express를 이용한 프로젝트의 구조를 살펴보려고 한다.

&nbsp; Elice 3기 백엔드를 배우는 주차이다. express를 배우는 중에 완성된 프로젝트 구조를 한 번 살펴보면 좋을 것 같다는 생각이 들어서 오늘의 주제로 가져왔다.

&nbsp; 자 그럼 코드를 통해 구조를 살펴보자!

<br>

# Express 구조! 코드로 뜯어보자!!

<br>

<br>

## 전체 구조 살펴보기

<br>

```cmd
📦express_project
 ┃
 ┣ 📂models
 ┃ ┣ 📂schemas
 ┃ ┃ ┣ 📂types
 ┃ ┃ ┃ ┗ 📜shortId.js
 ┃ ┃ ┗ 📜post.js
 ┃ ┗ 📜index.js
 ┃
 ┣ 📂public
 ┃ ┗ 📂stylesheet
 ┃ ┃ ┗ 📜style.css
 ┃
 ┣ 📂routes
 ┃ ┣ 📜index.js
 ┃ ┗ 📜posts.js
 ┃
 ┣ 📂views
 ┃ ┣ 📂post
 ┃ ┃ ┣ 📜edit.pug
 ┃ ┃ ┣ 📜list.pug
 ┃ ┃ ┗ 📜view.pug
 ┃ ┣ 📜error.pug
 ┃ ┣ 📜index.pug
 ┃ ┗ 📜layout.pug
 ┃
 ┗ 📜app.js
```

👉 &nbsp; 프로젝트의 전체적인 구조를 Tree로 나타냈다.

&nbsp; 이렇게 보면 복잡해 보이지만 차분하게 하나하나 살펴보면 그렇게 어렵진 않았다.

&nbsp; 자 그럼 세세하게 살펴보도록 하자.

<br>

## app.js

<br>

<br>

### app.js에서 필요한 모듈 가져오기

<br>

```js
// app.js

const createError = require('http-errors');
// 에러처리 관련

const express = require('express');
// 서버 관련

const path = require('path');
// 경로 관련

const cookieParser = require('cookie-parser');
// 인증 관련

const logger = require('morgan');
// 로그 관련

const mongoose = require('mongoose');
// DB관련

const dayjs = require('dayjs');
// 날짜 관련

const indexRouter = require('./routes');
// 라우터 관련
```

👉 &nbsp; app.js에서 필요한 모듈들을 가져왔다.

&nbsp; 한 번씩은 들어봤던 모듈들인 것 같다. 대략적으로 어떤 역할을 하는지는 알고 있으니 일단은 넘어가고, 나중에 실제 프로젝트에서 사용해야할 때 사용법과 자세한 내용들을 알아보도록 하겠다.

<br>

### 1. DB 연결

<br>

```js
// app.js

mongoose.connect('mongodb://localhost:27017/simple-board');
// MongoDB와 연결, 후에 추가적인 에러처리 필요

mongoose.connection.on('connected', () => {
    console.log('MongoDB Connected');
});
// MongoDB의 이벤트를 listen한다.
// DB와 연결이 되었을 때, 콜백 함수 실행
```

👉 &nbsp; mongoose를 이용하여 DB와 연결을 한다.

<br>

### 2. 서버 생성 및 세팅

<br>

```js
// app.js

const app = express();
// 서버 생성

app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'pug');
// 서버에 템플렛 엔진 연결

app.locals.formatDate = (date) => {
    return dayjs(date).format('YYYY-MM-DD HH:mm:ss');
};
// 날짜 포맷팅...?

app.use(logger('dev'));
// 개발 수준의 로거 설정

app.use(express.json());
app.use(express.urlencoded({ extended: false }));
// body parser 설정

app.use(cookieParser());
// 쿠키 parser 설정

app.use(express.static(path.join(__dirname, 'public')));
// 정적 파일을 제공할 폴더 설정
```

👉 &nbsp; 서버를 생성했다. 그리고 불러왔던 모듈들을 이용하여 서버에서 필요한 요소들을 셋업해줬다.

<br>

### 3. 라우팅 및 에러 처리

<br>

```js
// app.js

app.use('/', indexRouter);
// 라우팅 처리

app.use((req, res, next) => {
    next(createError(404));
});
// 라우터에서 정의한 url이 아닌 다른 url이 온다면
// 404 에러를 넘겨준다.
// ... 1

app.use((err, req, res, next) => {
    res.locals.message = err.message;
    // 에러 메세지를 응답에 설정해준다.

    res.locals.error = req.app.get('env') === 'development' ? err : {};
    // 요청을 보내온 곳..?의 개발환경이 개발이면
    // 메세지를 보여주고 아니면 안 보여준다.

    res.status(err.status || 500);
    // err.status에 값이 있으면 그 값을
    // 아니면 500 에러를 보내준다.
    res.render('error');
});

module.exports = app;
```

👉 &nbsp; 라우터 및 에러처리를 해줬다.

&nbsp; 라우팅에서 걸러지지 않고 ...1번에 도착하면 이는 클라이언트에서 잘못된 Url로 요청이 온 것이므로 404 에러를 next 메서드를 이용하여 에러처리 미들웨어로 보내준다.

<br>

## 라우터

<br>

```js
// app.js

app.use('/', indexRouter);

// ./routes/index.js

const { Router } = require('express');
const postsRouter = require('./posts');

const router = Router();

router.use('/posts', postsRouter);
// /posts로 시작되는 url은 postsRouter에서 처리한다.

router.get('/', (req, res) => {
    res.redirect('/posts');
});
// /로 오는 요청은 /posts로 다시 요청하도록 한다.
```

👉 &nbsp; 예제와는 다르게 indexRouter에서 모든 라우터를 다루려고 했다. 따라서 router.use를 이용하여 모든 라우팅 처리는 IndexRouter에서 시작되도록 했다.

```js
// posts.js

const { Router } = require('express');
const { Post } = require('../models');

const router = Router();

router.get('/', async (req, res, next) => {
    if (req.query.write) {
        res.render('post/edit');
        return;
    }
    // html의 form이 submit이 될 때,
    // /posts?write=true로 요청이 온다.
    // 그렇다면 위의 if문을 수행하여 edit.pug를 렌더링 해준다.

    const posts = await Post.find({});

    res.render('post/list', { posts });
});

router.post('/', async (req, res, next) => {
    const { title, content } = req.body;

    try {
        if (!title || !content) {
            throw new Error('제목과 내용을 입력해 주세요.');
        }

        const post = await Post.create({
            title,
            content,
        });

        res.redirect(`/posts/${post.shortId}`);
    } catch (err) {
        next(err);
    }
});

router.get('/:shortId', async (req, res, next) => {
    const { shortId } = req.params;

    const post = await Post.findOne({ shortId });

    if (req.query.edit) {
        res.render('post,edit', { post });
    }

    res.render('post/view', { post });
});

router.post('/:shortId', async (req, res, next) => {
    const { shortId } = req.params;

    const { title, content } = req.body;

    try {
        if (!title || !content) {
            throw new Error('제목과 내용을 입력 해 주세요');
        }

        await Post.updateOne(
            { shortId },
            {
                title,
                content,
            }
        );

        res.redirect(`/posts/${shortId}`);
    } catch (err) {
        next(err);
    }
});

router.delete('/:shortId', async (req, res, next) => {
    const { shortId } = req.params;

    try {
        await Post.deleteOne({ shortId });
    } catch (err) {
        next(err);
    }

    res.send('ok');
});

module.exports = router;
```

👉 &nbsp; ~~하나하나 다 주석을 달기엔 귀찮..으니까~~
<br>

종합해서 정리하면 html의 form과 button을 이용하여 여러 url로 요청과 응답을 주고 받는다.

👉 &nbsp; Mongoose의 ODM을 이용하여 CRUD를 하는 모습도 확인할 수 있다.

<br>

#

<br>
