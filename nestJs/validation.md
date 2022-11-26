<br>

## 🔎  Validation

<br>

👉  유효성 검사를 위한 라이브러리는 여러가지가 있는데, 여기서는 class-validator에 대해 알아보자.

<br>

```tsx
$ npm i --save class-validator class-transformer
```

<br>

위의 명령어를 통해 라이브러리를 설치하자.

 <br>

```tsx
// cat.schema.ts

// 필요한 유효성 검사 데코레이터를 가져온다.
import { IsEmail, IsNotEmpty, IsString } from 'class-validator';

export class Cat extends Document {
  @Prop({ required: true, unique: true })
  @IsEmail()
  @IsNotEmpty()
  email: string;

	...
}
```

<br>

하고자 하는 유효성 검사 데코레이터를 @Prop 데코레이터 아래에 작성해주면 된다.

 <br>

```tsx
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
	...
	app.useGlobalPipes(new ValidationPipe());
}
```

<br>

validation을 하기 위해서는 위처럼 app과 연결시켜야 한다.

 <br>
