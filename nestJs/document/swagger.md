<br>

# Swagger를 이용한 API 문서 만들기

<br>

## 🔎  Swagger란?

<br>

👉  **Swagger는 RESTful API를 설명하는 데 사용되는 언어 독립적인 정의 형식이다.**

→ 무슨 말이지…? 그냥 RESTful한 API를 설명하는 걸 도와주는 것이라고 이해했다.

<br>

**Nest는 데코레이터를 활용하여 이러한 스펙을 생성할 수 있는 전용 모듈을 제공한다.**

→ Nest의 방식인 데코레이터를 활용하여 이를 이용할 수 있다고 이해했다.

<br>

## 🔎  Swagger 설치 및 실행

<br>

```tsx
$ npm install --save @nestjs/swagger
```

<br>

위의 명령어를 통해 라이브러리를 설치한다.

설치 후, `main.ts` 에서 SwaggerModule을 사용한다.

<br>

```tsx
...
import {SwaggerModule, DocumentBuilder} from '@nestjs/swagger';

async function bootstrap(){
	const app = await NestFactory.create(AppModule);
	const config = new DocumentBuilder()
		.setTitle('Cats example')
		.setDescription('The cats API description')
		.setVersion('1.0')
		.addTag('cats')
		.build();
	const document = SwaggerModule.createDocument(app, config);
	SwaggerModule.setup('api', app, document);

	await app.listen(3000);
}

bootstrap();
```

<br>

자 이제 설정을 끝마쳤으니 서버를 실행해보자.

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/af196353-56f0-4040-bb0f-dcfb8b56c674/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221128T092037Z&X-Amz-Expires=86400&X-Amz-Signature=f458620c232592f12d28a3791b448546191265093ad4ce9a877b718592517a72&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

서버를 실행 후, SwaggerModule.setup에서 설정한 ‘api’ path로 접속하면
(e.g. localhost:3000/api)
위와 같은 화면이 나오게 된다. 컨트롤러에서 작성한 api들에 대한 정보가 잘 나와있다.

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0067d425-999b-4868-bf55-ccf740a84095/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221128T104029Z&X-Amz-Expires=86400&X-Amz-Signature=6d986564a63e46438447f8eae875475c3e7ed522fd5a6af35afafa996f50e9ff&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

하지만 api를 상세히 살펴보면 parameters나 responses 등 상세한 내용은 없는 걸 확인할 수 있다. 또한 `/cats` 는 singUp 기능을 하는데 이 문서 만으로는 어떤 역할을 하는지 파악하기 어렵다.

<br>

## 🔎  Swagger 문서를 채워보자!

<br>

```tsx
// cat.request.dto.ts

import { IsEmail, IsNotEmpty, IsString } from 'class-validator';
import { ApiProperty } from '@nestjs/swagger';

export class CatRequestDto {
  @ApiProperty({
    example: 'rkrkdldkd@naver.com',
    description: 'email',
    required: true,
  })
  @IsEmail()
  @IsNotEmpty()
  email: string;

  @ApiProperty()
  @IsString()
  @IsNotEmpty()
  password: string;

  @ApiProperty()
  @IsString()
  @IsNotEmpty()
  name: string;
}
```

<br>

👉  dto의 프로퍼티들에 @ApiProperty 데코레이터를 붙여줬다. 그 후 Swagger를 확인해보면

@ApiProperty의 signiture에 어떤 값을 넣냐에 따라 보이는 Example이 달라지는 걸 꼭 확인해보자!

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/85bc2cfb-6ccd-4f57-aa5a-4be865b31417/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221128T104113Z&X-Amz-Expires=86400&X-Amz-Signature=bd6c4fbbed4dd59189889062137317e88f8436d7da343e06f723240827650c94&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

위처럼 Request body가 추가된 걸 확인할 수 있다.

<br>

```tsx

@ApiOperation({ summary: '회원가입' })
@Post()
async signUp(@Body() body: CatRequestDto) {
	return await this.catsService.signUp(body);
}
```

<br>

@ApiOperation() 데코레이터를 사용하면

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/315315a7-4ece-4847-b6e2-1b5ef1f5662f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221128T104139Z&X-Amz-Expires=86400&X-Amz-Signature=1798c2ccd915a766bcca5b3cbd770786722fde8ee8b74fbde4577473212668e2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

해당 api가 어떤 역할을 하는지 나타내준다.

자 다음으로 response를 정의해주자.

<br>

```tsx
// cats.controller.ts

export class CatsController {

  constructor(private readonly catsService: CatsService) {}

	...

  @ApiResponse({
    status: 500,
    description: 'Server Error...',
  })
  @ApiResponse({
    status: 200,
    description: '성공',

		// ReadOnlyDto는 없기 때문에 새로 만들어줬다. 밑에서 설명하겠다.
    type: ReadOnlyDto,
  })
  @ApiOperation({ summary: '회원가입' })
  @Post()
  async signUp(@Body() body: CatRequestDto) {
    return await this.catsService.signUp(body);
  }

	...
}
```

<br>

Response가 성공일 때와 실패일 때의 경우를 모두 정의해줬다.
그리고 아직까지 ReadOnlyDto가 없기 때문에 새로 만든 후 정의해줬다.

<br>

```tsx
import { ApiProperty } from '@nestjs/swagger';

export class ReadOnlyDto {
  @ApiProperty({
    example: '1kd1w22s',
    required: true,
  })
  id: string;

  @ApiProperty({
    example: 'rkrkdldk@naver.com',
    required: true,
  })
  email: string;

  @ApiProperty({
    example: 'shin',
    required: true,
  })
  name: string;
}
```

<br>

ReadOnlyDto는 MongoDB의 id는 필요하고 password는 불필요하기 때문에 이를 가공해서 작성했다.

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4f8830f4-08b6-407e-89a3-fee89443e156/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221128T104155Z&X-Amz-Expires=86400&X-Amz-Signature=cddb6d34d8bc2bfa299e87973b6d6c7ef4a9acc4096ed769a737fe914f6330f7&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

그 결과 위처럼 문서가 잘 만들어진 모습을 확인할 수 있다.

<br>

## 🔎  상속을 통한 코드 리팩토링

<br>

👉  API 문서는 성공적으로 잘 만들었다. 하지만 Schema에 적혀있는 속성들을 DTO를 만들어서 쓸데없이 재사용을 하는 느낌이 있다.

따라서 이번 시간에는 상속을 통한 리팩토링을 해보도록 하겠다.

<br>

```tsx
// cats.schema.ts

@Schema(options)
export class Cat extends Document {
  @ApiProperty({
    example: 'rkrkdldkd@naver.com',
    required: true,
  })
  @Prop({ required: true, unique: true })
  @IsEmail()
  @IsNotEmpty()
  email: string;

  @ApiProperty({
    example: 'shin',
    required: true,
  })
  @Prop({ required: true })
  @IsString()
  @IsNotEmpty()
  name: string;

  @ApiProperty({
    example: '12345',
    required: true,
  })
  @Prop({ required: true })
  @IsString()
  @IsNotEmpty()
  password: string;

  @Prop()
  @IsString()
  imgUrl: string;

  readonly readOnlyData: {
    id: string;
    email: string;
    name: string;
  };
}
```

<br>

기존에 DTO에 적었던 문서 관련 데코레이터들을 Schema에 작성해준다.

<br>

```tsx
// cats.request.dto.ts

import { Cat } from '../cats.schema';

export class CatRequestDto extends Cat {}

---

// cats.response.dto.ts

import { ApiProperty } from '@nestjs/swagger';
import { Cat } from '../cats.schema';

export class ReadOnlyDto extends Cat {
  @ApiProperty({
    example: '1kd1w22s',
    required: true,
  })
  id: string;
}
```

<br>

그 후 스키마를 상속하여 재사용성을 높였다.

하지만 이렇게 하면 한 가지의 문제가 발생한다.

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7126f7ca-6c85-4d0e-8ddc-54530831ce9b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221128T104211Z&X-Amz-Expires=86400&X-Amz-Signature=f7048489af920edce5301555deeb693087933bd97e26a2b26114b24bcfb70c00&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

response 객체에 password가 나오기 때문이다.

<br>

```tsx
// cats.response.dto.ts

import { ApiProperty, PickType } from '@nestjs/swagger';
import { Cat } from '../cats.schema';

export class ReadOnlyDto extends PickType(Cat, ['email', 'name'] as const) {
  @ApiProperty({
    example: '1kd1w22s',
    required: true,
  })
  id: string;
}
```

<br>

이를 해결하기 위해 swagger에서 제공해주는 PickType 메서드를 이용하면 된다.

이걸 봤을 때 TS의 Pick이 생각이 났다. 내부적으로 pick이 사용되는 걸 확인해서 뿌듯했다 하핳…

암튼 지정한 속성만을 뽑아서 상속받을 수 있게 해준다고 생각하면 될 것 같다.

덕분에 성공적으로 원하는 값만 가져올 수 있게 되었다.
