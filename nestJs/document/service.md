<br>

# 🔎  NestJS 공식문서 Service 공부하기

<br>

## 🔎 Providers

<br>

👉 Nest classes는 services, repositories, factories, helpers 등을 공급자로 취급할 수 있다.
이러한 provider는 의존성을 주입할 수 있다. 이것이 의미하는 것은 다양한 관계를 만들 수 있다는 것이다. 이러한 기능은 Nest의 런타임 시스템에 위임 받을 수 있다…?

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2b488ca9-f012-45c1-94dd-da385f59c0af/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221124T072135Z&X-Amz-Expires=86400&X-Amz-Signature=970e9a5fa2d32eb1ec3d0b0f8d422fddbf58ed1a33f554a1f668bc836f631363&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

우리는 이전 챕터에서 CatsController를 만들었다. Controllers는 HTTP requests를 다뤄야 하고 providers에 더 복잡한 일들을 위임해야 한다. Providers는 모듈에서 공급자로 선언되는 일반 JS classes이다.

즉, 컨트롤러는 자신이 할 일에만 집중하고 더 복잡한 것들에 대해선 책임을 지지 않고, Providers가 처리할 수 있도록 위임해야 한다.

<br>

> Nest는 SOLID 원칙을 따르는 것이 좋다.

<br>

## 🔎 Services

<br>

👉 자 이제 CastsService를 만들 것이다. 이 서비스는 데이터의 저장과 검색을 할 책임이 있고, CatsController에서 사용될 수 있도록 설계되었다. 따라서 공급자로 정의하기에 좋다.

<br>

```jsx
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```

<br>

> CLI를 사용하여 서비스를 생성하려면 $ nest g service cats 명령을 실행하면 된다.

<br>

눈여겨볼 점은 @Injectable() 데코레이터이다. 이는 메타데이터를 첨부한다. 이것이 뜻하는 건 CatsService는 Nest IoC 컨테이너에서 관리할 수 있는 클래스라는 것이다.
그건 그렇고 이 예제는 다음과 같은 Cat 인터페이스도 사용한다.

 <br>

```jsx
export interface Cat {
  name: string;
  age: number;
  breed: string;
}
```

<br>

이제 고양이를 검색하는 서비스 클래스가 있으므로 CatsController 내에서 사용하겠다.

<br>

```jsx
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```

<br>

CatsService는 클래스의 생성자를 통해 주입되었다. 그리고 이때 private syntax가 붙어있는 걸 주목하자. 이를 사용하면 동일한 위치에서 catsService 멤버를 즉시 선언하고 초기화할 수 있다.

즉, Nest에서 알아서 선언과 초기화를 해주니까 그냥 바로 사용하면 된다는 뜻!

<br>

## 🔎 Dependency injection

<br>

👉 Nest는 DI라고 알려진 강력한 디자인 패턴을 기반으로 구축되었다.

Nest에서는 TS의 기능덕분에 종속성이 유형별로 해결되기 때문에 매우 쉽게 종속성을 관리할 수 있다. 예를 들면 Nest는 CatsService 인스턴스를 생성하고 반환하여 catsService를 해결한다. 이 종속성은 해결되어 컨트롤러의 생성자로 전달된다.

<br>

```jsx
constructor(private catsService: CatsService) {}
```

<br>

아직 잘 이해가 되진 않는다. 하지만 일단 Module의 Providers 배열 안에 들어간 Providers는 Nest가 인스턴스를 생성하고 반환하여 그 인스턴스를 컨트롤러의 생성자로 전달한다고 이해했다.

<br>

## 🔎 Scopes

<br>

👉 공급자는 애플리케이션의 생명 주기와 동기화된 생명주기(”범위”)를 갖는다.
애플리케이션이 부트스트랩되면 모든 종속성을 해결해야 하므로 모든 Providers를 인스턴스화 해야한다. 마찬가지로 애플리케이션이 종료되면 각 Providers는 소멸된다.

그러나 따로 Providers의 생명주기를 조작할 수 있는 방법도 있다. 이는 나중에 알아보자!!

<br>

## 🔎 Custom providers

<br>

👉 Nest에는 Providers의 관계를 해결하는 IoC 컨테이너가 내장되어 있다. 이는 위에서 설명한 DI의 기반이 되지만 지금까지의 설명보다 훨씬 강력하다. Providers를 정의하는 방법에는 여러가지가 있다.

<br>

👉 Nest에는 Providers의 관계를 해결하는 IoC 컨테이너가 내장되어 있다. 이는 위에서 설명한 DI의 기반이 되지만 지금까지의 설명보다 훨씬 강력하다. Providers를 정의하는 방법에는 여러가지가 있다.
→ 일반 값, 클래스 및 비동기 또는 동기 팩토리

<br>

이것도 추후에 알아보도록 하자!!

<br>

## 🔎 Optional providers

<br>

👉 경우에 따라서 해결할 필요가 없는 종속성이 있을 수 있다. 이러한 경우 Provider가 없어도 오류가 발생하지 않으므로 종속성이 선택 사항이 된다.

Provider가 선택 사항일을 나타내려면 `@Optional()` 데코레이터를 생성자’s signature에 넣어주면 된다.

<br>

```jsx
import { Injectable, Optional, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  constructor(@Optional() @Inject('HTTP_OPTIONS') private httpClient: T) {}
}
```

<br>

Custom provider를 사용하고 있기 때문에 ‘HTTP_OPTIONS’라는 Custom token을 사용하고 있다.

이 부분 역시 나중에 더 자세히 알아보도록 하자!

<br>

## 🔎 Provider registration

<br>

👉 이제 provider(CatsService)를 정의했고, 해당 서비스의 소비자(CatsController)가 있으므로 Injection을 수행하기 위해 service를 등록할 필요가 있다.
이를 위해서는 module file(app.module.ts)에서 `@Module()` 데코레이터의 providers 배열에 서비스를 더해야한다.

<br>

```jsx
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';
import { CatsService } from './cats/cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```

<br>

Nest는 이제 CatsController 클래스의 종석성을 해결할 수 있다.

이제 디렉토리의 구조는 다음과 같다.

<br>

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3d9d3348-9956-4580-9cf2-369d7614222f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221124T072155Z&X-Amz-Expires=86400&X-Amz-Signature=6e893b46e11342b7a418f6436350a9b7a9e26e052d88310ed7473539dd55e851&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)
