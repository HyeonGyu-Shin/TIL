<br>

## ๐ย  Validation

<br>

๐ย  ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ์ํ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ ์ฌ๋ฌ๊ฐ์ง๊ฐ ์๋๋ฐ, ์ฌ๊ธฐ์๋ class-validator์ ๋ํด ์์๋ณด์.

<br>

```tsx
$ npm i --save class-validator class-transformer
```

<br>

์์ ๋ช๋ น์ด๋ฅผ ํตํด ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ค์นํ์.

 <br>

```tsx
// cat.schema.ts

// ํ์ํ ์ ํจ์ฑ ๊ฒ์ฌ ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ๊ฐ์ ธ์จ๋ค.
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

ํ๊ณ ์ ํ๋ ์ ํจ์ฑ ๊ฒ์ฌ ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ @Prop ๋ฐ์ฝ๋ ์ดํฐ ์๋์ ์์ฑํด์ฃผ๋ฉด ๋๋ค.

 <br>

```tsx
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
	...
	app.useGlobalPipes(new ValidationPipe());
}
```

<br>

validation์ ํ๊ธฐ ์ํด์๋ ์์ฒ๋ผ app๊ณผ ์ฐ๊ฒฐ์์ผ์ผ ํ๋ค.

 <br>
