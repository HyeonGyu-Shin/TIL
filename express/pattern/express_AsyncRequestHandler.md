<br>

# Async Request Handler...? 그게 뭔데??

<br>

👉 &nbsp; express에서 사용하는 패턴 중 하나이다.

&nbsp; request handler(비동기 함수)에서 오류를 처리하기 위한 방법은 다음의 2가지와 같다.

1. promise().catch(next)

2. async function, try ~ catch, next

&nbsp; async의 비동기 처리는 매우 편리하지만, 매번 try ~ catch 구문을 작성하는 것은 귀찮고 실수하기가 쉽다.

&nbsp; 이러한 문제를 해결하기 위해
함수로 감싸서 try ~ catch, next를 자동으로 할 수 있도록 하는 원리이다.

<br>

# 자 그럼 Async Request Handler! 살펴봅시다!!

<br>

```js

const asyncHandler = requestHandler => {
    return async (req, res, next) => {
        try{
            await requestHandler(req, res);
        }catch(err){
            next(err);
        }
    }
}

---

router.get('/', asyncHandler(
    async (req, res) => {
        const posts = await Posts.find({});

        if(posts.length < 1){
            throw new Error('Not Found');
        }

        res.render('posts/list', {posts});
    }
));

```

👉 &nbsp; asyncHandler 함수는 매개변수로 넘어오는 함수를 try~ catch문으로 감싸서 실행해주는 함수라고 생각하면 된다.

&nbsp; 따라서 requestHandler 함수에서 에러가 발생한다면 catch 문에서 감지되어 오류처리 미들웨어로 전달된다.

<br>

# 오...! 다른 곳에서도 활용할 수 있겠는걸??

<br>

👉 &nbsp; 아직 뭘 만들어야 할지는 생각이 나지 않지만, 추후에 프로젝트를 진행할 때 위와 같은 handler를 만들어서 개발 생산성을 높이면 좋을 것 같다는 생각이 들었다.
