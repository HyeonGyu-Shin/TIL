<br>

## ๐ย  Transaction์ ์ํ ACID ์์น

<br>

### โ๏ธย  **Atomicity**

<br>

๐ย  transaction์ ๋ชจ๋ ๋ฐ์๋๊ฑฐ๋ ๋ชจ๋ ์คํจํ์ฌ์ผ ํ๋ค.

์ฆ, All or Noting์ ๊ฐ๋์ผ๋ก ์์์ ์ผ๋ถ๋ถ๋ง ์คํํ์ง ์๋๋ค๋ ๊ฑธ ์๋ฏธํ๋ค.

<br>

### โ๏ธย  **Consistency**

<br>

๐ย  transaction์ด ์๋ฃ๋๋ฉด DB๋ ์ผ๊ด๋ ์ํ๋ฅผ ์ ์งํ์ฌ์ผ ํ๋ค.

์๋ฅผ ๋ค์ด, ์ก๊ธ ์ฒ๋ฆฌ ์์์ ํ๋ค๋ฉด ๊ธ์ก์ ํด๋นํ๋ ๋ฐ์ดํฐ๋ Number ํ์์ ๊ฐ์ง๋ค.
์ด๋ ๋ฐ์ดํฐ๊ฐ ๊ฐ์๊ธฐ String ํ์์ผ๋ก ๋ฐ๋์ง ์๋๋ค๋ ๊ฑธ ์๋ฏธํ๋ค.

์ฆ, ์ฒ๋ฆฌ ์  ํ ๋ชจ๋ ๊ฐ์ ๋ฐ์ดํฐ ํ์์ ์ ์งํ๋ค๋ ๊ฑธ ์๋ฏธํ๋ค.

<br>

### โ๏ธย  **Isolation**

<br>

๐ย  Transaction์ด ์ผ์ด๋  ๊ฒฝ์ฐ ํด๋น ๋ฐ์ดํฐ๋ฅผ ๊ฒฉ๋ฆฌ์์ผ์ ํ์ฌ์ transaction์ด ๋๋ ํ ๋ค์ transaction์ด ์คํ๋๋๋ก ํด์ค๋ค.

์ฆ, transaction ๋ผ๋ฆฌ๋ ์๋ก ๊ฐ์ญํ์ง ์๋๋ค๋ ๊ฑธ ์๋ฏธํ๋ค.

<br>

### โ๏ธย  **Durability**

<br>

๐ย  ์ฑ๊ณต์ ์ผ๋ก ์ํ๋ transaction์ ์์ํ ๋ฐ์๋๋ค.

<br>

## ๐ย  MongoDB์์ transaction ์ ์ฉํ๊ธฐ

<br>

```tsx
// startSession ๋ฉ์๋๋ฅผ ๋ถ๋ฌ์์ผ ํ๋ค.
const {startSession} = require('mongoose');

commentRouter.post('/'), async (req, res) => {
	const session = await startSession();
	let comment;

	try{
		await session.withTransaction(async () => {

			...

			const [blog, user] = await Promise.all([
				// ๋ฐ์ดํฐ๋ฅผ ๋ถ๋ฌ์ฌ ๋ session์ ๊ผญ ๋ฃ์ด์ค์ผ ํ๋ค.
				Blog.findById(blogId, {}, {session},
				User.findById(userId, {}, {session},
			])

			comment = new Comment({...});

			await Promise.all([
				comment.save({session}),

				// blog๋ findById๋ฅผ ํตํด ๋ถ๋ฌ์จ ๊ฐ์ด๋ฏ๋ก session ์๋ต ๊ฐ๋ฅํ๋ค.
				blog.save().
			])

		}

	}catch(err){

	}
}

```

<br>

๐ย  session.withTransaction() ์์์ ์ด๋ฃจ์ด์ง๋ ๋ชจ๋  ์ฒ๋ฆฌ๋ค์ session์ ๋ฐ์์ ํด์ฃผ๋ ๊ฒ์ด๋ค.
๊ทธ ํ session.withTransaction()์ return ๊ฐ์ด await๋ฅผ ํตํด ๋ฐํ์ด ๋๋ฉด, ๊ทธ ๋ DB์ ๋ฐ์์ด ๋๋ค.

<br>

๐ย  session.withTransaction()์ ์ฝ๋ฐฑํจ์๊ฐ ์คํจ์ ์๋ฌ๋ฅผ ๋์ง์ง ์๊ณ , ์ผ์  ์๊ฐ ๊ฐ๊ฒฉ์ผ๋ก ์ฑ๊ณตํ  ๋๊น์ง?(๊ณ์ ํ์ง ์์ ๊ฒ ๊ฐ๋ค.) ๋ค์ ์ฝ๋ฐฑํจ์๋ฅผ ์คํํ๋ค.

๊ทธ๋ ๋ค๋ฉด ๋ค์ ๋ฐ๋ณตํ์ง ์๊ณ  ๋๋ด๊ณ  ์ถ์ ๋ ์ด๋ป๊ฒ ํด์ผํ ๊น??
session.abortTransaction()์ ์ฌ์ฉํ๋ฉด ๋๋ค!!

<br>

```tsx
const [blog, user] = await Promise.all([

				Blog.findById(blogId, {}, {session},
				User.findById(userId, {}, {session},
]);

// ์ด๋ ๊ฒ ์๋ฌ๋ฅผ ์ฒ๋ฆฌํ๊ฒ ๋๋ฉด ๊ทธ ์ ๊น์ง ์ด๋ฃจ์ด์ก๋ ์ฒ๋ฆฌ๋ค์ ๋ค ์คํ์ด ๋ ํ ํจ์๊ฐ ๋๋  ๋ฟ์ด๋ค.
// ์ฆ, ํธ๋์ญ์์ด ์ทจ์๋๋ ๊ฑด ์๋๋ค.
if(!blog || !user) return res.status(400).send({err: 'blog or user does not exist'});

// ํ์ง๋ง abortTransactgion()์ ๊ฒฝ์ฐ ํธ๋์ญ์์ด ์ทจ์๊ฐ ๋๋ค.
// ๊ทธ๋ฅ withTransaction() ์์ ๋ด์ฉ๋ค ๋ค ์ทจ์๋๋ ๊ฑฐ์ผ~~~
await session.abortTransaction()
```

<br>

Transaction์ API๊ฐ ๋ฌด๊ฑฐ์์ง๊ธฐ ๋๋ฌธ์ ๋ชจ๋  ๋ฉ์๋์ ์ฌ์ฉํ  ํ์๋ ์๊ณ , ์ค์ํ ๊ณณ์์ ์ฌ์ฉํ๋ฉด ๋๋ค!

<br>

## ๐ย  MongoDB์ ์ฐ์ฐ์๋ฅผ ํตํด Atomicํ๊ฒ ์๋ฐ์ดํธ๋ฅผ ํด์ค๋ค.

<br>

```tsx
const blog = await Blog.findById(blogId);

blog.commentsCount++;
blog.comments.push(comment);
if (blog.commentsCount > 3) blog.comments.shift();

await Promise.all([comment.save(), blog.save()]);
```

<br>

```tsx

await Promise.all([
	comment.save(),
	Blog.updateOne(
		{_id, blogId},
		{$inc: {commentsCount: 1},
	  {$push:{comments:{$each:[comment], $slice: -3}}
	);
]);
```

<br>

๐ย  ์์ ๋ ์ฝ๋๋ ๊ฐ์ ๋์์ ์ํํ๋ค.

ํ์ง๋ง ์๋์ ์ฝ๋๋ฅผ ์ฌ์ฉํ  ๊ฒฝ์ฐ DB๊ฐ Transaction์ ๋ณด์ฅํด์ฃผ๊ธฐ ๋๋ฌธ์ ๊ฐ๋ฐ์๊ฐ ACID๋ฅผ ์ง์ผ์ผํ  ์ฑ์์์ ๋ฒ์ด๋๊ฒ ํด์ค๋ค.

๋ฐ๋ผ์ ๋๋๋ก์ด๋ฉด ์๋์ ์ฝ๋ ์คํ์ผ๋ก ์ฝ๋๋ฅผ ์์ฑํ์!

์ด ๋ถ๋ถ์ ๊ณต๋ถํ๋ฉด์ ์๋ฟ์ง ์์๋ NoSQL์ ์ฅ์ ์ ๋๋ผ๊ฒ ๋์๋ค.
โ ์์ ๋ก์ด ๋ฌธ์ ๊ตฌ์กฐ, ๋ฌธ์ ๋ด์ฅ์ ํตํด DB์ ์ฐ์ฐ์๋ฅผ ์์ฝ๊ฒ ํ์ฉํ  ์ ์๋ค.
