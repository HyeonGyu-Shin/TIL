<br>

# ๐ย  Sharded cluster

<br>

## ๐ย  Vertical Scale

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/63d12cc2-134f-45db-9eec-184d447ce2a0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221207%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221207T134831Z&X-Amz-Expires=86400&X-Amz-Signature=e6bd9eb25b8de9f8f4721e7e31055feeb14da385049a7f0e30c319204eefe27d&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

๐ย  ์์ง์  ํ์ฅ์ ํ์ฌ DB์ ์ฌ์์ ๋์ด๋ ๊ฒ์ด๋ค.
DB ์ฌ์์ ๋์ธ๋ค ํด๋ ๋ชจ๋  ๋ฐ์ดํฐ๋ Primary DB ์์ ์กด์ฌํ๋ค.

<br>

## ๐ย  Vertical Scale

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7dfe65c7-f59f-4b87-9abc-ca2f55702eb4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221207%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221207T134845Z&X-Amz-Expires=86400&X-Amz-Signature=dc9f0e91931c4b354b048f4bf2eb210a2e6557e49525ea6453e668a1ec6ba3d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

๐ย  ์ํ์  ํ์ฅ์ ๊ทธ๋ฅ DB๋ฅผ ๋ฐ๋ก ๋๋ ๊ฒ์ด๋ค. ๊ฐ์ฅ ํฐ ์ฅ์ ์ ํ๊ณ๊ฐ ์๋ค๋ ๊ฒ์ด๋ค.

Replica Set์ ์ฌ๋ฌ ๊ฐ ๋๋ ๊ฑธ Sharded Cluster๋ผ๊ณ  ํ๋ค.

<br>

# ๐ย  ์ ๋ฆฌ

<br>

- RDB๊ฐ ์ํ์  ํ์ฅ์ด ์ด๋ ค์ด ์ด์ ์ ๋ํด ์ ์ ์๊ฒ ๋์๋ค.
  - RDB๋ ์ ๊ทํ์ ๊ด๊ณ๋ฅผ ํตํด ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ค๋๋ฐ, ๋ง์ฝ DB๊ฐ ์ฌ๋ฌ๊ฐ๋ผ๋ฉด Consistency์ ์ํฅ์ ๋ฐ์ ์ ์๊ณ , ๋ง๋ฅ ์ฑ๋ฅ์ด ์ข์์ง๋ค๋ ๋ณด์ฅ์ ๋ชป ํ  ์ ์๋ค.
  - RDB๋ Auto Increment ์ฆ int์ธ id๋ฅผ ์ฌ์ฉํ๋๋ฐ, ๋ง์ฝ DB๊ฐ ์ฌ๋ฌ๊ฐ๋ผ๋ฉด id๊ฐ ๊ผฌ์ผ ์ ์๋ค. ์ด์ ๋ฐํด MongoDB๋ ObjectId๋ผ๋ ๋๋ค๊ฐ์ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ ๋ ์ ๋ฆฌํ๋ค.
