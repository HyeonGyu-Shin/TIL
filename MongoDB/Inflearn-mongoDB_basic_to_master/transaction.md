<br>

## 🔎  Transaction을 위한 ACID 원칙

<br>

### ✏️  **Atomicity**

<br>

👉  transaction은 모두 반영되거나 모두 실패하여야 한다.

즉, All or Noting의 개념으로 작업의 일부분만 실행하지 않는다는 걸 의미한다.

<br>

### ✏️  **Consistency**

<br>

👉  transaction이 완료되면 DB는 일관된 상태를 유지하여야 한다.

예를 들어, 송금 처리 작업을 한다면 금액에 해당하는 데이터는 Number 타입을 가진다.
이때 데이터가 갑자기 String 타입으로 바뀌지 않는다는 걸 의미한다.

즉, 처리 전 후 모두 같은 데이터 타입을 유지한다는 걸 의미한다.

<br>

### ✏️  **Isolation**

<br>

👉  Transaction이 일어날 경우 해당 데이터를 격리시켜서 현재의 transaction이 끝난 후 다음 transaction이 실행되도록 해준다.

즉, transaction 끼리는 서로 간섭하지 않는다는 걸 의미한다.

<br>

### ✏️  **Durability**

<br>

👉  성공적으로 수행된 transaction은 영원히 반영된다.

<br>

## 🔎  MongoDB에서 transaction 적용하기

<br>

```tsx
// startSession 메서드를 불러와야 한다.
const {startSession} = require('mongoose');

commentRouter.post('/'), async (req, res) => {
	const session = await startSession();
	let comment;

	try{
		await session.withTransaction(async () => {

			...

			const [blog, user] = await Promise.all([
				// 데이터를 불러올 때 session을 꼭 넣어줘야 한다.
				Blog.findById(blogId, {}, {session},
				User.findById(userId, {}, {session},
			])

			comment = new Comment({...});

			await Promise.all([
				comment.save({session}),

				// blog는 findById를 통해 불러온 값이므로 session 생략 가능하다.
				blog.save().
			])

		}

	}catch(err){

	}
}

```

<br>

👉  session.withTransaction() 안에서 이루어지는 모든 처리들은 session에 반영을 해주는 것이다.
그 후 session.withTransaction()의 return 값이 await를 통해 반환이 되면, 그 때 DB에 반영이 된다.

<br>

👉  session.withTransaction()은 콜백함수가 실패시 에러를 던지지 않고, 일정 시간 간격으로 성공할 때까지?(계속 하진 않을 것 같다.) 다시 콜백함수를 실행한다.

그렇다면 다시 반복하지 않고 끝내고 싶을 땐 어떻게 해야할까??
session.abortTransaction()을 사용하면 된다!!

<br>

```tsx
const [blog, user] = await Promise.all([

				Blog.findById(blogId, {}, {session},
				User.findById(userId, {}, {session},
]);

// 이렇게 에러를 처리하게 되면 그 전까지 이루어졌던 처리들은 다 실행이 된 후 함수가 끝날 뿐이다.
// 즉, 트랜잭션이 취소되는 건 아니다.
if(!blog || !user) return res.status(400).send({err: 'blog or user does not exist'});

// 하지만 abortTransactgion()의 경우 트랜잭션이 취소가 된다.
// 그냥 withTransaction() 안의 내용들 다 취소되는 거야~~~
await session.abortTransaction()
```

<br>

Transaction은 API가 무거워지기 때문에 모든 메서드에 사용할 필요는 없고, 중요한 곳에서 사용하면 된다!

<br>

## 🔎  MongoDB의 연산자를 통해 Atomic하게 업데이트를 해준다.

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

👉  위의 두 코드는 같은 동작을 수행한다.

하지만 아래의 코드를 사용할 경우 DB가 Transaction을 보장해주기 때문에 개발자가 ACID를 지켜야할 책임에서 벗어나게 해준다.

따라서 되도록이면 아래의 코드 스타일로 코드를 작성하자!

이 부분을 공부하면서 와닿지 않았던 NoSQL의 장점을 느끼게 되었다.
→ 자유로운 문서 구조, 문서 내장을 통해 DB의 연산자를 손쉽게 활용할 수 있다.
