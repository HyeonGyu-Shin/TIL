<br>

# Mongoose ODM에 대해서

<br>

👉 &nbsp; mongoose라는 typeODM에 대해 배워보려고 한다.

&nbsp; typeODM을 사용하면 보다 쉽게 DB의 쿼리를 CRUD 할 수 있기 때문이다.

&nbsp; Java를 사용할 땐 JPA, Node를 사용할 땐 Sequelize를 사용했었다. 이번에 새롭게 MongoDB를 배우는데 mongoose라는 ODM을 사용한다고 한다.

&nbsp; 자 그럼 배워보도록 하자!

<br>

# Mongoose 배워보자!!

<br>

## Mongoose ODM 사용 순서

<br>

1. 스키마 정의
2. 모델 만들기
3. 데이터 베이스 연결
4. 모델 사용

<br>

### 1. 스키마 정의

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

👉 &nbsp; Collection에 저장될 Document의 스키마를 Code-Level에서 관리할 수 있도록 Schema를 작성할 수 있다.

&nbsp; 다양한 형식을 미리 미리 지정하여 생성, 수정 작업 시 데이터 형식을 체크해주는 기능을 제공한다. (Like TypeScript...)

&nbsp; timestamps 옵션을 사용하면 생성, 수정 시간을 자동으로 기록해준다.

<br>

### 2. 모델 만들기

<br>

```js
// ./models/index.js

const mongoose = require('mongoose');

const PostSchema = require('./schemas/board');

exports.Post = mongoose.model('Post', PostSchema);
```

👉 &nbsp; 작성된 스키마를 mongoose에서 사용할 수 있는 모델로 만들어야 한다.

&nbsp;모델의 이름을 지정하여 해당 이름으로 모델을 호출할 수 있다.

<br>

### 3. 데이터 베이스 연결

<br>

```js
// index.js

const mongoose = require('mongoose');
const {Post} = require('./models);

mongoose.connect('mongodb://localhost:27017/myapp');

// Post 바로 사용 가능
```

👉 &nbsp; connect 함수를 이용하여 간단하게 데이터베이스에 연결할 수 있다.

&nbsp; mongoose는 자동으로 연결을 관리해줘서 직접 연결 상태를 체크하지 않아도, 모델 사용시 연결 상태를 확인하여 사용이 가능할 때 작업을 한다고 한다.

<br>

### 4. 모델 사용

<br>

👉 &nbsp; 작성된 모델을 이용하여 CRUD를 수행할 수 있다.

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
// 쿼리 예제

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

// {key: value}로 exact match
// $lt, $lte, $gt, $gte를 사용하여 range query 작성 가능
// $in을 사용하여 다중 값으로 검색
// $or를 사용하여 다중 조건 검색

Person.find({name: ['elice', 'bob']});
// {name: { $in: ['elice', 'bob']}}

// Mongoose는 쿼리 값으로 배열이 주어지면 자동으로 $in 쿼리를 생성해 줌
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

// mongoose의 update는 기본적으로 $set operator를 사용하여
// Document를 통째로 변경하지 않는다.
// 즉, 수정할 부분만 변경한다.

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

## Mongoose 위치 정하기

<br>

👉 &nbsp; 일반적으로 models 디렉터리에 Schema와 Model을 같이 위치

&nbsp; app 객체는 어플리케이션 시작을 의미하는 부분이므로 해당 부분에 데이터베이스 연결을 명시하는 mongoose.connect를 위치

<br>

## Mongoose ODM 커넥션 이벤트

<br>

```js

mongoose.connect('---');

mongoose. connection.on('connected', () => {});
// 연결 완료

mongoose. connection.on('disconnected', () => {});
// 연결이 끊김

mongoose. connection.on('reconnected', () => {});
// 재연결 완료

mongoose. connection.on('reconnectedFailed', () => {});

// 재연결 시도 횟수 초과
```







