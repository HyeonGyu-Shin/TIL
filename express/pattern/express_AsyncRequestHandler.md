<br>

# Async Request Handler...? ๊ทธ๊ฒ ๋ญ๋ฐ??

<br>

๐ &nbsp; express์์ ์ฌ์ฉํ๋ ํจํด ์ค ํ๋์ด๋ค.

&nbsp; request handler(๋น๋๊ธฐ ํจ์)์์ ์ค๋ฅ๋ฅผ ์ฒ๋ฆฌํ๊ธฐ ์ํ ๋ฐฉ๋ฒ์ ๋ค์์ 2๊ฐ์ง์ ๊ฐ๋ค.

1. promise().catch(next)

2. async function, try ~ catch, next

&nbsp; async์ ๋น๋๊ธฐ ์ฒ๋ฆฌ๋ ๋งค์ฐ ํธ๋ฆฌํ์ง๋ง, ๋งค๋ฒ try ~ catch ๊ตฌ๋ฌธ์ ์์ฑํ๋ ๊ฒ์ ๊ท์ฐฎ๊ณ  ์ค์ํ๊ธฐ๊ฐ ์ฝ๋ค.

&nbsp; ์ด๋ฌํ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด
ํจ์๋ก ๊ฐ์ธ์ try ~ catch, next๋ฅผ ์๋์ผ๋ก ํ  ์ ์๋๋ก ํ๋ ์๋ฆฌ์ด๋ค.

<br>

# ์ ๊ทธ๋ผ Async Request Handler! ์ดํด๋ด์๋ค!!

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

๐ &nbsp; asyncHandler ํจ์๋ ๋งค๊ฐ๋ณ์๋ก ๋์ด์ค๋ ํจ์๋ฅผ try~ catch๋ฌธ์ผ๋ก ๊ฐ์ธ์ ์คํํด์ฃผ๋ ํจ์๋ผ๊ณ  ์๊ฐํ๋ฉด ๋๋ค.

&nbsp; ๋ฐ๋ผ์ requestHandler ํจ์์์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค๋ฉด catch ๋ฌธ์์ ๊ฐ์ง๋์ด ์ค๋ฅ์ฒ๋ฆฌ ๋ฏธ๋ค์จ์ด๋ก ์ ๋ฌ๋๋ค.

<br>

# ์ค...! ๋ค๋ฅธ ๊ณณ์์๋ ํ์ฉํ  ์ ์๊ฒ ๋๊ฑธ??

<br>

๐ &nbsp; ์์ง ๋ญ ๋ง๋ค์ด์ผ ํ ์ง๋ ์๊ฐ์ด ๋์ง ์์ง๋ง, ์ถํ์ ํ๋ก์ ํธ๋ฅผ ์งํํ  ๋ ์์ ๊ฐ์ handler๋ฅผ ๋ง๋ค์ด์ ๊ฐ๋ฐ ์์ฐ์ฑ์ ๋์ด๋ฉด ์ข์ ๊ฒ ๊ฐ๋ค๋ ์๊ฐ์ด ๋ค์๋ค.
