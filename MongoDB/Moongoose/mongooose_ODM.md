<br>

# Mongoose ODM์ ๋ํด์

<br>

๐ &nbsp; mongoose๋ผ๋ typeODM์ ๋ํด ๋ฐฐ์๋ณด๋ ค๊ณ  ํ๋ค.

&nbsp; typeODM์ ์ฌ์ฉํ๋ฉด ๋ณด๋ค ์ฝ๊ฒ DB์ ์ฟผ๋ฆฌ๋ฅผ CRUD ํ  ์ ์๊ธฐ ๋๋ฌธ์ด๋ค.

&nbsp; Java๋ฅผ ์ฌ์ฉํ  ๋ JPA, Node๋ฅผ ์ฌ์ฉํ  ๋ Sequelize๋ฅผ ์ฌ์ฉํ์๋ค. ์ด๋ฒ์ ์๋กญ๊ฒ MongoDB๋ฅผ ๋ฐฐ์ฐ๋๋ฐ mongoose๋ผ๋ ODM์ ์ฌ์ฉํ๋ค๊ณ  ํ๋ค.

&nbsp; ์ ๊ทธ๋ผ ๋ฐฐ์๋ณด๋๋ก ํ์!

<br>

# Mongoose ๋ฐฐ์๋ณด์!!

<br>

## Mongoose ODM ์ฌ์ฉ ์์

<br>

1. ์คํค๋ง ์ ์
2. ๋ชจ๋ธ ๋ง๋ค๊ธฐ
3. ๋ฐ์ดํฐ ๋ฒ ์ด์ค ์ฐ๊ฒฐ
4. ๋ชจ๋ธ ์ฌ์ฉ

<br>

### 1. ์คํค๋ง ์ ์

<br>

```js
// ./models/schemas/board.js

const { Schema } = require('mongoose');

const PostShema = new Schema(
    {
        title: String,
        content: String,
    },
    {
        timestamps: true,
    }
);
```

๐ &nbsp; Collection์ ์ ์ฅ๋  Document์ ์คํค๋ง๋ฅผ Code-Level์์ ๊ด๋ฆฌํ  ์ ์๋๋ก Schema๋ฅผ ์์ฑํ  ์ ์๋ค.

&nbsp; ๋ค์ํ ํ์์ ๋ฏธ๋ฆฌ ๋ฏธ๋ฆฌ ์ง์ ํ์ฌ ์์ฑ, ์์  ์์ ์ ๋ฐ์ดํฐ ํ์์ ์ฒดํฌํด์ฃผ๋ ๊ธฐ๋ฅ์ ์ ๊ณตํ๋ค. (Like TypeScript...)

&nbsp; timestamps ์ต์์ ์ฌ์ฉํ๋ฉด ์์ฑ, ์์  ์๊ฐ์ ์๋์ผ๋ก ๊ธฐ๋กํด์ค๋ค.

<br>

### 2. ๋ชจ๋ธ ๋ง๋ค๊ธฐ

<br>

```js
// ./models/index.js

const mongoose = require('mongoose');

const PostSchema = require('./schemas/board');

exports.Post = mongoose.model('Post', PostSchema);
```

๐ &nbsp; ์์ฑ๋ ์คํค๋ง๋ฅผ mongoose์์ ์ฌ์ฉํ  ์ ์๋ ๋ชจ๋ธ๋ก ๋ง๋ค์ด์ผ ํ๋ค.

&nbsp;๋ชจ๋ธ์ ์ด๋ฆ์ ์ง์ ํ์ฌ ํด๋น ์ด๋ฆ์ผ๋ก ๋ชจ๋ธ์ ํธ์ถํ  ์ ์๋ค.

<br>

### 3. ๋ฐ์ดํฐ ๋ฒ ์ด์ค ์ฐ๊ฒฐ

<br>

```js
// index.js

const mongoose = require('mongoose');
const {Post} = require('./models);

mongoose.connect('mongodb://localhost:27017/myapp');

// Post ๋ฐ๋ก ์ฌ์ฉ ๊ฐ๋ฅ
```

๐ &nbsp; connect ํจ์๋ฅผ ์ด์ฉํ์ฌ ๊ฐ๋จํ๊ฒ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ฐ๊ฒฐํ  ์ ์๋ค.

&nbsp; mongoose๋ ์๋์ผ๋ก ์ฐ๊ฒฐ์ ๊ด๋ฆฌํด์ค์ ์ง์  ์ฐ๊ฒฐ ์ํ๋ฅผ ์ฒดํฌํ์ง ์์๋, ๋ชจ๋ธ ์ฌ์ฉ์ ์ฐ๊ฒฐ ์ํ๋ฅผ ํ์ธํ์ฌ ์ฌ์ฉ์ด ๊ฐ๋ฅํ  ๋ ์์์ ํ๋ค๊ณ  ํ๋ค.

<br>

### 4. ๋ชจ๋ธ ์ฌ์ฉ

<br>

๐ &nbsp; ์์ฑ๋ ๋ชจ๋ธ์ ์ด์ฉํ์ฌ CRUD๋ฅผ ์ํํ  ์ ์๋ค.

<br>

1. Create -> create

<br>

```js
// index.js

const { Post } = require('./models');

async function main(){
    const created = await Post.create({
        title: 'first title',
        content: 'second title',
    });

    const mulipleCreated = await Post.create([
        item1, item2
    ]);
}
```

<br>

2. Read -> find, findById, findOne

<br>


```js
// index.js

const {Post} = require('./models');

async function main(){
    const listPost = await Post.find(query);
    const onePost = await Post.findOne(query);
    const postById = await Post.findById(id);
}
```

```js
// ์ฟผ๋ฆฌ ์์ 

Person.find({
    name: 'gyu',
    age: {
        $lt: 20,
        $gte: 10,
    },
    languages: {
        $in: ['ko', 'en'],
    },
    $or:[
        {status: 'ACTIVE'},
        {isFresh: true},
    ],
});

// {key: value}๋ก exact match
// $lt, $lte, $gt, $gte๋ฅผ ์ฌ์ฉํ์ฌ range query ์์ฑ ๊ฐ๋ฅ
// $in์ ์ฌ์ฉํ์ฌ ๋ค์ค ๊ฐ์ผ๋ก ๊ฒ์
// $or๋ฅผ ์ฌ์ฉํ์ฌ ๋ค์ค ์กฐ๊ฑด ๊ฒ์

Person.find({name: ['elice', 'bob']});
// {name: { $in: ['elice', 'bob']}}

// Mongoose๋ ์ฟผ๋ฆฌ ๊ฐ์ผ๋ก ๋ฐฐ์ด์ด ์ฃผ์ด์ง๋ฉด ์๋์ผ๋ก $in ์ฟผ๋ฆฌ๋ฅผ ์์ฑํด ์ค
```

<br>

3. Update -> updateOne, updateMany, findByIdAndUpdate, findOneAndUpdate

<br>

```js
// index.js

const updateResult = await Post.updateOne(query, {...});

const updateResults = await Post.updateMany(query, {...});

const postById = await Post.findByIdAndUpdate(id, {...});

const onePost = await Post.findOneAndUpdate(query, {...});

// mongoose์ update๋ ๊ธฐ๋ณธ์ ์ผ๋ก $set operator๋ฅผ ์ฌ์ฉํ์ฌ
// Document๋ฅผ ํต์งธ๋ก ๋ณ๊ฒฝํ์ง ์๋๋ค.
// ์ฆ, ์์ ํ  ๋ถ๋ถ๋ง ๋ณ๊ฒฝํ๋ค.

```

<br>

4. Delete -> deleteOne, deleteMany, findByIdAndDelete, findoneAndDelete

<br>

```js
// index.js

async function main(){
    const deleteResult = await Post.deleteOne(query);

    const deleteResults = await Post.deleteMany(query);

    const onePost = await Post.findOneAndDelete(query);

    const postById = await Post.findByIdAndDelete(query);
}
```

<br>

## Mongoose ์์น ์ ํ๊ธฐ

<br>

๐ &nbsp; ์ผ๋ฐ์ ์ผ๋ก models ๋๋ ํฐ๋ฆฌ์ Schema์ Model์ ๊ฐ์ด ์์น

&nbsp; app ๊ฐ์ฒด๋ ์ดํ๋ฆฌ์ผ์ด์ ์์์ ์๋ฏธํ๋ ๋ถ๋ถ์ด๋ฏ๋ก ํด๋น ๋ถ๋ถ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ฐ๊ฒฐ์ ๋ช์ํ๋ mongoose.connect๋ฅผ ์์น

<br>

## Mongoose ODM ์ปค๋ฅ์ ์ด๋ฒคํธ

<br>

```js

mongoose.connect('---');

mongoose. connection.on('connected', () => {});
// ์ฐ๊ฒฐ ์๋ฃ

mongoose. connection.on('disconnected', () => {});
// ์ฐ๊ฒฐ์ด ๋๊น

mongoose. connection.on('reconnected', () => {});
// ์ฌ์ฐ๊ฒฐ ์๋ฃ

mongoose. connection.on('reconnectedFailed', () => {});

// ์ฌ์ฐ๊ฒฐ ์๋ ํ์ ์ด๊ณผ
```







