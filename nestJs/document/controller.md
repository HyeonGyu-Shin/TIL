<br>

# 🔎  NestJS 공식문서 Controller 공부하기

<br>

👉  컨트롤러는 들어오는 요청을 처리하고 클라이언트에 응답을 반환하는 역할을 한다.

<br>

![Untitled](https://docs.nestjs.com/assets/Controllers_1.png)

<br>

컨트롤러의 목적은 애플리케이션에 대한 특정 요청을 수신하는 것이다. 라우터를 통해 어떤 컨트롤러가 어떤 요청을 받아야 하는지 제어한다.

기본 컨트롤러를 만들기 위해 클래스와 데코레이터를 사용한다.

데코레이터는 클래스를 필수 메타데이터와 연결하고 Nest가 라우팅 맵을 생성할 수 있도록 한다.
→ 요청을 해당 컨트롤러에 연결

<br>

> 유효성 검사가 내장된 CRUD 컨트롤러를 빠르게 생성하려면 CLI의 CRUD 생성기를 사용할 수 있다. `nest g resource [name].`

<br>

## 🔎 라우팅

<br>

👉 기본 컨트롤러를 정의할 때는 @Controller() 데코레이터가 필요하다.
또한 경로 접두사를 사용하면 관련 경로 집합을 쉽게 그룹화하고 반복 코드를 최소화 할 수 있다.

이렇게만 말하면 이해하기 어려우니 코드를 통해 살펴보도록 하자.

<br>

```jsx
import { Controller, Get } from '@nestjs/common';

// @Controller의 매개변수에 넣은 string 값이 경로 접두사이다.
// 즉 아래의 의미는 /cats로 요청이 들어왔을 때 실행할 컨트롤러란 뜻이다.
@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'this action returns all cats';
  }
}
```

<br>

> CLI를 사용하여 컨트롤러를 생성하려면 $ nest g controller cats 명령을 실행하면 된다.

<br>

`findAll()` 은 Nest에게 HTTP 요청에 대한 특정 endpoint에 대한 핸들러를 생성하도록 지시한다. 이때, endpoint는 route path에 해당한다.

그렇다면 route path는 무엇일까? route path는 controller에 선언된 경로 접두사와 http 요청 메서드의 데코레이션에 작성된 경로에 의해 결정되는 최종 path라고 생각하면 된다.
예를 들어, 위에선 @Controller 데코에 cats을 선언했고, @Get 데코에 아무런 값도 넣지 않았으므로 Nest는 `Get /cats` 란 요청에 대해 `findAll()`이란 핸들러를 매핑시켜줄 것이다.

<br>

<aside>
💡 controller path prefix + http method decorator’s path => route path

next는 위에서 생성된 route path로 핸들러를 매핑, 요청이 들어오면 실행한다.

</aside>

<br>

`findAll()` 메서드는 200 상태 코드와 관련 응답을 반환한다. 이 경우에는 문자열일 뿐이다. 왜 그런지에 대해 알아보려면 Nest가 응답을 조작하기 위해 두 가지의 다른 옵션을 사용한다는 개념을 알아야 한다.

<br>

1. standard

   1. Built-in된 메서드를 사용할 때, request handler가 JS의 oject나 array를 return한다면, return값은 JSON으로 직렬화 된다. 그러나 JS의 원시타입을 return한다면 직렬화되지 않는다. 이렇게 하면 응답 처리가 간단해진다. 값을 반환하기만 하면 Nest가 나머지를 처리한다.

   <br>

   2. 또한 201을 사용하는 Post를 제외하면, response’s status code는 항상 200이 default이다. `@HttpCode()` 데코레이터를 추가하여 이러한 동작을 쉽게 변경할 수 있다.

   <br>

   ```jsx
   @Post()
   @HttpCode(204)
   create() {
     return 'This action adds a new cat';
   }
   ```

   <br>

2. Library-specific

<br>

1.  Express와 같은 라이브러리의 응답 객체를 사용할 수 있다. method handler signiture에 있는 `@Res()` 가 그 예이다( e.g., `findAll(@Res() response`).

   <br>

2.  이러한 접근은 응답 객체에 의해 노출된 응답 처리 메서드를 사용할 수 있게 해준다.예를 들면 `response.status(200).send()` 와 같이 Express에서 사용하는 응답 처리코드를 사용할 수 있다.

   <br>

<aside>
💡  Nest는 `@Res()` 또는 `@Next`를 사용할 때를 감지하여 library-specific option을 선택했음을 나타낸다. 만약 두 접근 방식을 모두 사용했다면, Standard approach는 자동으로 disabled 된다.

<br>

만약 두 가지 모두를 사용하고 싶다면 `@Res({ passthrough: true})` 처럼 passthrough option을 true로 명시해줘야 한다.

</aside>

<br>

## 🔎 요청 객체

<br>

👉 Handler는 종종 request에 대해 detail하게 접근해야 하는데, Nest는 Express의 request object에 접근할 수 있게 해준다. 이를 위해 handler’s signiture에 `@Req` 데코레이터를 추가해주면 된다.

<br>

```jsx
import { Controller, Get, Req } from '@nestjs/common';
import { Request } from 'express';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Req() request: Request): string {
    return 'This action returns all cats';
  }
}
```

<br>

> request: Request와 같은 express의 타이핑을 활용하려면 `@types/express` package를 설치하면 된다.

<br>

요청 객체는 HTTP 요청, query string, parameters, HTTP headers and body 등에 대한 정보를 나타낸다. 하지만 대부분의 경우 우리는 이러한 모든 정보가 필요하진 않다.
따라서 우리는 `@Body()` 또는 `@Query()`와 같은 전용 데코레이터를 사용하여 필요한 정보만 가져올 수 있다.
→ 더 많은 데코레이터에 대한 정보는 [여기](https://docs.nestjs.com/controllers)를 참고하자!

Nest는 method handler에 @Res() 또는 @Response()와 같은 데코레이터가 주입되면 해당 handler를 **Library-specific mode**로 전환하고 응답 관리를 한다. 이렇게 되면 response object를 호출하여 일종의 응답을 발행해줘야 한다.(e.g., `res.json(…)` or `res.send(…)`)
그렇지 않으면 HTTP 서버는 중단된다.

<br>

> 커스텀 데코레이터도 만들 수 있다는데 이건 추후에 알아보도록 하자!!

<br>

## 🔎 자원

<br>

👉 처음에 우리는 Get /cats에 대한 handler 를 정의했다. 이번에는 Post handler를 만들어보자.

<br>

```jsx
import { Controller, Get, Post } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Post()
  create(): string {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
```

<br>

간단했다! 위에서 봤듯이 Nest는 standard TTP methods를 위한 데코레이터를 제공한다.
→ @Get(), @Post(), @Put(), @Patch(), @Delete(), @Options(), @Head(), @All(),

<br>

## 🔎 Route wildcards

<br>

👉 패턴 기반의 경로 역시 제공된다. 예를 들어 asterisk(\*)는 모든 문자열과 일치한다.

<br>

```jsx
@Get('ab*cd')
findAll() {
  return 'This route uses a wildcard';
}
```

<br>

`'ab*cd'` 는  `abcd`, `ab_cd`, `abecd` 와 매칭된다. 그 외에 `?`, `+`, `*`,  `()` 역시 사용 가능하다.

<br>

## 🔎 상태 코드

<br>

👉 위에서 다뤘듯이, Post 요청을 제외한 상태 코드는 항상 200이다. 우리는 쉽게 Hendler level에서 `@HttpCode(…)` 데코레이터를 이용하여 상태 코드를 변화시킬 수 있다.

<br>

```jsx
@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```

<br>

> HttpCode는 `@nextjs/common` package로부터 import 된다.

<br>

종종 상태코드는 다양한 요소에 의해 달라질 수 있다. 이러한 경우 library-specific response object를 이용하여 해결할 수 있다.
→ error 상황, throw an exception 등등

<br>

## 🔎 Headers

<br>

👉 커스텀 응답 헤더를 지정하려면 `@Header()` 데코레이터 또는 library-specific response object(res.header())를 이용하면 된다.

<br>

```jsx
@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```

<br>

> Header는 `@nextjs/common` package로부터 import 된다.

<br>

## 🔎 Redirection

<br>

👉 특정 URL로 redirect를 하고싶다면 `@Redirect()` 데코레이터를 쓰거나 library-specific response object(res.redirect())를 이용하면 된다.

`@Redirect()` 는 url과 statusCode 두 가지의 인수를 받는데, 둘 다 optional이다. 이 때의 기본값은 302이다.

<br>

```jsx
@Get()
@Redirect('https://nestjs.com', 301)
```

<br>

때때로 동적으로 결정되어야 할 때가 있다. 이럴 경우 return 값을 이용하여 @Redirect()를 override 할 수 있다.

<br>

```jsx
@Get('docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs(@Query('version') version) {
  if (version && version === '5') {
    return { url: 'https://docs.nestjs.com/v5/' };
  }
}
```

<br>
