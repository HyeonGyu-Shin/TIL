<br>

# What is Pagenation...?

<br>

👉 &nbsp; Pagenation은 전체 페이지를 개별 페이지로 나누는 프로세스이다. - 위키백과

&nbsp; 데이터가 많아지면 한 페이지의 목록에 모든 데이터를 표현하기 어렵다.

&nbsp; 이러한 문제를 해결하기 위해 page 단위로 데이터를 나누고 DB 혹은 서버에서 현재 페이지의 데이터만 넘겨준다.

&nbsp; Pagenation을 통해 화면에 적당한 수의 데이터를 보여주고 이를 통해 UX의 향상이란 효과를 얻을 수 있다.

<br>

# Pagenation, 어떻게 하는지 알아봅시다!

<br>

👉 &nbsp; pagenation에 필요한 정보는 일반적으로 query parameter로 전달받는다.

```js

// /posts?page=1&perPage=10

router.get(... => {
    const page = Number(req.query.page || 1);
    // 현재 페이지

    const perPage = Number(req,query,perPage || 10);
    // 페이지 당 게시글 수

...

```

<br>

## MongoDB에서는 어떻게 사용해야 할까??

<br>

👉 &nbsp; MongoDB에서는 limit과 skip을 사용하여 pagination 구현이 가능하다.

-   limit - 검색 결과 수 제한
-   skip - 검색 시 포함하지 않을 데이터 수

&nbsp; 또한 데이터의 순서가 유지될 수 있도록 sort를 사용하는걸 권장한다.

```js

router.get(... => {
    ...

    const total = await Post.countDocument({});
    // countDocument 메서드는
    // collection에 있는 게시글의 수를 반환해준다.

    const posts = await Post.find({})
    .sort({createdAt: -1})
    .skip(perPage * (page - 1))
    .limit(perPage);


    const totalPage = Math.ceil(total / perPage);

})

```

👉 &nbsp; 코드를 살펴보자.

1. sort({createdAt: -1}) - 생성 날짜를 기준으로 정렬을 한다.

2. skip(perPage \* (page - 1)) - 이전 페이지의 데이터는 건너뛰어야 한다.

3. limit - perPage의 수만큼의 데이터를 가져와야 한다.

4. Math.ceil(total / perPage) - 전체 페이지를 계산한다.

<br>

# 정리

<br>

👉 &nbsp; DB에서 데이터를 가공하는 방법에 대해 잘 배운 것 같다. 예전에 DB에서의 연산보다 서버에서 연산하는 것이 더 빠르다는 말을 들은 것 같은데 다음에 더 자세히 알아보도록 해야겠다.

&nbsp; 또한, DB에 대해 더 공부해야겠다는 걸 느꼈다.
