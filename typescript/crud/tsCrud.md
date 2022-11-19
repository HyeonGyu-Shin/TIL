<br>

### ✏️  TS 세팅하기

<br>

```tsx
tsc -- init
```

<br>

👉 위의 명령어를 통해 tsconfig.json 파일을 생성했다.

<br>

```tsx
{
  "compilerOptions": {
    "module": "system",
    "noImplicitAny": true,
    "removeComments": true,
    "preserveConstEnums": true,
    "outFile": "../../built/local/tsc.js",
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

<br>

실제론 더 길고 주석처리가 많이 되어있지만 일단 위와 비슷한 내용의 설정들이 담긴 json 파일이 생성된다.

include와 exclude, outDir 정도만 수정하고 설정 세팅은 끝냈다.

<br>

### ✏️  필요한 라이브러리 생성

<br>

```tsx
"@types/express": "^4.17.14",  // TS 타입 판별을 위해 필요한 라이브러리...?
"@types/node": "^18.11.9",  // TS 타입 판별을 위해 필요한 라이브러리...?
"nodemon": "^2.0.20",
"tsc-watch": "^5.0.3",  // 자동으로 tsc를 해주는 라이브러리
"typescript": "^4.8.4"
```

<br>

👉  필요한 라이브러리들을 받아왔다.

<br>

```tsx
"scripts": {
    "start": "tsc-watch --onSuccess \"nodemon ./dist/app.js\" "
},
```

<br>

👉  자동으로 JS로 컴파일하고 서버를 시작하기 위해 위와 같은 script를 추가했다.

<br>

### ✏️  app.js

<br>

```tsx
import express from 'express';

import { todoRouter } from './routes/index.js';

const app = express();

app.use('/api', todoRouter);

app.all('/', (req, res) => {
  res.send('hihi');
});

app.listen(5500, () => {
  console.log('server started port: 5500');
});
```

<br>

👉  컴파일러가 거의 대부분 타입을 추론해줘서 app.js에선 손을 댈 게 없었다.

<br>

### ✏️  db/index.js

<br>

```tsx
export interface DbData {
  id: number;
  content: string;
  isFinished: boolean;
}

class DataBase {
  db: DbData[] = [{ id: 0, content: 'hello', isFinished: false }];
  idCount: number = 1;
}
const Db = new DataBase();
export { Db };
```

<br>

👉  따로 DB를 이용하진 않았고, 임시로 배열을 이용하여 DB처럼 이용했다.

<br>

### ✏️  todoModel.js

<br>

```tsx
import { Db, DbData } from '../index.js';

interface TodoModelInterface {
  create(todoInfo: { content: string }): {
    id: number;
    content: string;
    isFinished: boolean;
  };

  find(id: number): DbData;
  //   update(id: number): void;
  //   delete(id: number): void;
}

class TodoModel implements Partial<TodoModelInterface> {
  create(todoInfo: { content: string }) {
    const _id = Db.idCount++;

    const newTodo = {
      id: _id,
      content: todoInfo.content,
      isFinished: false,
    };

    Db.db.push(newTodo);
    return newTodo;
  }

  find(id: number) {
    const foundTodo = Db.db.find((v) => v.id === id);

    if (typeof foundTodo === 'undefined') {
      return {
        id: -1,
        content: '없음',
        isFinished: true,
      };
    }

    return foundTodo;
  }
  //   update(id: number) {}
  //   delete(id: number) {}
}

const todoModel = new TodoModel();

export { todoModel };
```

<br>

👉  interface를 처음으로 적용해봤다.

<br>

미리 메서드들을 정의한 후 상속(구현이라고 해야하나…?)을 받아서 메서드를 작성했다.
find 부분에서 undefined에 대한 처리를 깜빡했는데 컴파일러가 에러를 통해 이 부분을 잡아줬다.

말로만 듣던 TS의 이점을 몸소 깨닫게 되었다.

<br>

### ✏️  todoRouter.js

<br>

```tsx
import { Router } from 'express';
import { Db } from '../db/index.js';
import { todoModel } from '../db/model/TodoModel.js';

const router = Router();

router.all('/', (req, res) => {
  console.log(Db.db);
  res.send('here is /api');
});

router.all('/create', (req, res) => {
  const createdTodo = todoModel.create({ content: 'hello' });
  res.send(`data created ${createdTodo}`);
});

router.all('/find', (req, res) => {
  const arr = Db.db;
  console.log(arr);
  res.send(`Db data is  ${arr}`);
});

router.post('/todo', (req, res, next) => {
  const createdTodo = todoModel.create({ content: 'hello' });
  console.log(createdTodo);
});

export { router as todoRouter };
```

<br>

👉  마찬가지로 라우터들은 컴파일러가 자동으로 타입을 추론해줬다.

todoModel의 메서드에 파라미터를 전달하는 부분에서도 컴파일러의 도움을 많이 받았다.

<br>

## 📌  정리

<br>

👉  처음엔 막막했지만 간단한 CRUD API를 만드는 과정을 통해서 약간은 TS와 친해진 것 같다.

다음에는 테스트 부분과 로그인, DB 연동 등을 구현해보면서 공부해야겠다!
