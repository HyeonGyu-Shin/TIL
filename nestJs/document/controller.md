<br>

# ๐ย  NestJS ๊ณต์๋ฌธ์ Controller ๊ณต๋ถํ๊ธฐ

<br>

๐ย  ์ปจํธ๋กค๋ฌ๋ ๋ค์ด์ค๋ ์์ฒญ์ ์ฒ๋ฆฌํ๊ณ  ํด๋ผ์ด์ธํธ์ ์๋ต์ ๋ฐํํ๋ ์ญํ ์ ํ๋ค.

<br>

![Untitled](https://docs.nestjs.com/assets/Controllers_1.png)

<br>

์ปจํธ๋กค๋ฌ์ ๋ชฉ์ ์ ์ ํ๋ฆฌ์ผ์ด์์ ๋ํ ํน์  ์์ฒญ์ ์์ ํ๋ ๊ฒ์ด๋ค. ๋ผ์ฐํฐ๋ฅผ ํตํด ์ด๋ค ์ปจํธ๋กค๋ฌ๊ฐ ์ด๋ค ์์ฒญ์ ๋ฐ์์ผ ํ๋์ง ์ ์ดํ๋ค.

๊ธฐ๋ณธ ์ปจํธ๋กค๋ฌ๋ฅผ ๋ง๋ค๊ธฐ ์ํด ํด๋์ค์ ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ฌ์ฉํ๋ค.

๋ฐ์ฝ๋ ์ดํฐ๋ ํด๋์ค๋ฅผ ํ์ ๋ฉํ๋ฐ์ดํฐ์ ์ฐ๊ฒฐํ๊ณ  Nest๊ฐ ๋ผ์ฐํ ๋งต์ ์์ฑํ  ์ ์๋๋ก ํ๋ค.
โ ์์ฒญ์ ํด๋น ์ปจํธ๋กค๋ฌ์ ์ฐ๊ฒฐ

<br>

> ์ ํจ์ฑ ๊ฒ์ฌ๊ฐ ๋ด์ฅ๋ CRUD ์ปจํธ๋กค๋ฌ๋ฅผ ๋น ๋ฅด๊ฒ ์์ฑํ๋ ค๋ฉด CLI์ CRUD ์์ฑ๊ธฐ๋ฅผ ์ฌ์ฉํ  ์ ์๋ค. `nest g resource [name].`

<br>

## ๐ ๋ผ์ฐํ

<br>

๐ ๊ธฐ๋ณธ ์ปจํธ๋กค๋ฌ๋ฅผ ์ ์ํ  ๋๋ @Controller() ๋ฐ์ฝ๋ ์ดํฐ๊ฐ ํ์ํ๋ค.
๋ํ ๊ฒฝ๋ก ์ ๋์ฌ๋ฅผ ์ฌ์ฉํ๋ฉด ๊ด๋ จ ๊ฒฝ๋ก ์งํฉ์ ์ฝ๊ฒ ๊ทธ๋ฃนํํ๊ณ  ๋ฐ๋ณต ์ฝ๋๋ฅผ ์ต์ํ ํ  ์ ์๋ค.

์ด๋ ๊ฒ๋ง ๋งํ๋ฉด ์ดํดํ๊ธฐ ์ด๋ ค์ฐ๋ ์ฝ๋๋ฅผ ํตํด ์ดํด๋ณด๋๋ก ํ์.

<br>

```jsx
import { Controller, Get } from '@nestjs/common';

// @Controller์ ๋งค๊ฐ๋ณ์์ ๋ฃ์ string ๊ฐ์ด ๊ฒฝ๋ก ์ ๋์ฌ์ด๋ค.
// ์ฆ ์๋์ ์๋ฏธ๋ /cats๋ก ์์ฒญ์ด ๋ค์ด์์ ๋ ์คํํ  ์ปจํธ๋กค๋ฌ๋ ๋ป์ด๋ค.
@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'this action returns all cats';
  }
}
```

<br>

> CLI๋ฅผ ์ฌ์ฉํ์ฌ ์ปจํธ๋กค๋ฌ๋ฅผ ์์ฑํ๋ ค๋ฉด $ nest g controller cats ๋ช๋ น์ ์คํํ๋ฉด ๋๋ค.

<br>

`findAll()` ์ Nest์๊ฒ HTTP ์์ฒญ์ ๋ํ ํน์  endpoint์ ๋ํ ํธ๋ค๋ฌ๋ฅผ ์์ฑํ๋๋ก ์ง์ํ๋ค. ์ด๋, endpoint๋ route path์ ํด๋นํ๋ค.

๊ทธ๋ ๋ค๋ฉด route path๋ ๋ฌด์์ผ๊น? route path๋ controller์ ์ ์ธ๋ ๊ฒฝ๋ก ์ ๋์ฌ์ http ์์ฒญ ๋ฉ์๋์ ๋ฐ์ฝ๋ ์ด์์ ์์ฑ๋ ๊ฒฝ๋ก์ ์ํด ๊ฒฐ์ ๋๋ ์ต์ข path๋ผ๊ณ  ์๊ฐํ๋ฉด ๋๋ค.
์๋ฅผ ๋ค์ด, ์์์  @Controller ๋ฐ์ฝ์ cats์ ์ ์ธํ๊ณ , @Get ๋ฐ์ฝ์ ์๋ฌด๋ฐ ๊ฐ๋ ๋ฃ์ง ์์์ผ๋ฏ๋ก Nest๋ `Get /cats` ๋ ์์ฒญ์ ๋ํด `findAll()`์ด๋ ํธ๋ค๋ฌ๋ฅผ ๋งคํ์์ผ์ค ๊ฒ์ด๋ค.

<br>

<aside>
๐ก controller path prefix + http method decoratorโs path => route path

next๋ ์์์ ์์ฑ๋ route path๋ก ํธ๋ค๋ฌ๋ฅผ ๋งคํ, ์์ฒญ์ด ๋ค์ด์ค๋ฉด ์คํํ๋ค.

</aside>

<br>

`findAll()` ๋ฉ์๋๋ 200 ์ํ ์ฝ๋์ ๊ด๋ จ ์๋ต์ ๋ฐํํ๋ค. ์ด ๊ฒฝ์ฐ์๋ ๋ฌธ์์ด์ผ ๋ฟ์ด๋ค. ์ ๊ทธ๋ฐ์ง์ ๋ํด ์์๋ณด๋ ค๋ฉด Nest๊ฐ ์๋ต์ ์กฐ์ํ๊ธฐ ์ํด ๋ ๊ฐ์ง์ ๋ค๋ฅธ ์ต์์ ์ฌ์ฉํ๋ค๋ ๊ฐ๋์ ์์์ผ ํ๋ค.

<br>

1. standard

   1. Built-in๋ ๋ฉ์๋๋ฅผ ์ฌ์ฉํ  ๋, request handler๊ฐ JS์ oject๋ array๋ฅผ returnํ๋ค๋ฉด, return๊ฐ์ JSON์ผ๋ก ์ง๋ ฌํ ๋๋ค. ๊ทธ๋ฌ๋ JS์ ์์ํ์์ returnํ๋ค๋ฉด ์ง๋ ฌํ๋์ง ์๋๋ค. ์ด๋ ๊ฒ ํ๋ฉด ์๋ต ์ฒ๋ฆฌ๊ฐ ๊ฐ๋จํด์ง๋ค. ๊ฐ์ ๋ฐํํ๊ธฐ๋ง ํ๋ฉด Nest๊ฐ ๋๋จธ์ง๋ฅผ ์ฒ๋ฆฌํ๋ค.

   <br>

   2. ๋ํ 201์ ์ฌ์ฉํ๋ Post๋ฅผ ์ ์ธํ๋ฉด, responseโs status code๋ ํญ์ 200์ด default์ด๋ค. `@HttpCode()` ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ถ๊ฐํ์ฌ ์ด๋ฌํ ๋์์ ์ฝ๊ฒ ๋ณ๊ฒฝํ  ์ ์๋ค.

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

1.  Express์ ๊ฐ์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ ์๋ต ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํ  ์ ์๋ค. method handler signiture์ ์๋ `@Res()` ๊ฐ ๊ทธ ์์ด๋ค( e.g., `findAll(@Res() response`).

   <br>

2.  ์ด๋ฌํ ์ ๊ทผ์ ์๋ต ๊ฐ์ฒด์ ์ํด ๋ธ์ถ๋ ์๋ต ์ฒ๋ฆฌ ๋ฉ์๋๋ฅผ ์ฌ์ฉํ  ์ ์๊ฒ ํด์ค๋ค.์๋ฅผ ๋ค๋ฉด `response.status(200).send()` ์ ๊ฐ์ด Express์์ ์ฌ์ฉํ๋ ์๋ต ์ฒ๋ฆฌ์ฝ๋๋ฅผ ์ฌ์ฉํ  ์ ์๋ค.

   <br>

<aside>
๐ก  Nest๋ `@Res()` ๋๋ `@Next`๋ฅผ ์ฌ์ฉํ  ๋๋ฅผ ๊ฐ์งํ์ฌ library-specific option์ ์ ํํ์์ ๋ํ๋ธ๋ค. ๋ง์ฝ ๋ ์ ๊ทผ ๋ฐฉ์์ ๋ชจ๋ ์ฌ์ฉํ๋ค๋ฉด, Standard approach๋ ์๋์ผ๋ก disabled ๋๋ค.

<br>

๋ง์ฝ ๋ ๊ฐ์ง ๋ชจ๋๋ฅผ ์ฌ์ฉํ๊ณ  ์ถ๋ค๋ฉด `@Res({ passthrough: true})` ์ฒ๋ผ passthrough option์ true๋ก ๋ช์ํด์ค์ผ ํ๋ค.

</aside>

<br>

## ๐ ์์ฒญ ๊ฐ์ฒด

<br>

๐ Handler๋ ์ข์ข request์ ๋ํด detailํ๊ฒ ์ ๊ทผํด์ผ ํ๋๋ฐ, Nest๋ Express์ request object์ ์ ๊ทผํ  ์ ์๊ฒ ํด์ค๋ค. ์ด๋ฅผ ์ํด handlerโs signiture์ `@Req` ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ถ๊ฐํด์ฃผ๋ฉด ๋๋ค.

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

> request: Request์ ๊ฐ์ express์ ํ์ดํ์ ํ์ฉํ๋ ค๋ฉด `@types/express` package๋ฅผ ์ค์นํ๋ฉด ๋๋ค.

<br>

์์ฒญ ๊ฐ์ฒด๋ HTTP ์์ฒญ, query string, parameters, HTTP headers and body ๋ฑ์ ๋ํ ์ ๋ณด๋ฅผ ๋ํ๋ธ๋ค. ํ์ง๋ง ๋๋ถ๋ถ์ ๊ฒฝ์ฐ ์ฐ๋ฆฌ๋ ์ด๋ฌํ ๋ชจ๋  ์ ๋ณด๊ฐ ํ์ํ์ง ์๋ค.
๋ฐ๋ผ์ ์ฐ๋ฆฌ๋ `@Body()` ๋๋ `@Query()`์ ๊ฐ์ ์ ์ฉ ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ฌ์ฉํ์ฌ ํ์ํ ์ ๋ณด๋ง ๊ฐ์ ธ์ฌ ์ ์๋ค.
โ ๋ ๋ง์ ๋ฐ์ฝ๋ ์ดํฐ์ ๋ํ ์ ๋ณด๋ [์ฌ๊ธฐ](https://docs.nestjs.com/controllers)๋ฅผ ์ฐธ๊ณ ํ์!

Nest๋ method handler์ @Res() ๋๋ @Response()์ ๊ฐ์ ๋ฐ์ฝ๋ ์ดํฐ๊ฐ ์ฃผ์๋๋ฉด ํด๋น handler๋ฅผ **Library-specific mode**๋ก ์ ํํ๊ณ  ์๋ต ๊ด๋ฆฌ๋ฅผ ํ๋ค. ์ด๋ ๊ฒ ๋๋ฉด response object๋ฅผ ํธ์ถํ์ฌ ์ผ์ข์ ์๋ต์ ๋ฐํํด์ค์ผ ํ๋ค.(e.g., `res.json(โฆ)` or `res.send(โฆ)`)
๊ทธ๋ ์ง ์์ผ๋ฉด HTTP ์๋ฒ๋ ์ค๋จ๋๋ค.

<br>

> ์ปค์คํ ๋ฐ์ฝ๋ ์ดํฐ๋ ๋ง๋ค ์ ์๋ค๋๋ฐ ์ด๊ฑด ์ถํ์ ์์๋ณด๋๋ก ํ์!!

<br>

## ๐ ์์

<br>

๐ ์ฒ์์ ์ฐ๋ฆฌ๋ Get /cats์ ๋ํ handler ๋ฅผ ์ ์ํ๋ค. ์ด๋ฒ์๋ Post handler๋ฅผ ๋ง๋ค์ด๋ณด์.

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

๊ฐ๋จํ๋ค! ์์์ ๋ดค๋ฏ์ด Nest๋ standard TTP methods๋ฅผ ์ํ ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ ๊ณตํ๋ค.
โ @Get(), @Post(), @Put(), @Patch(), @Delete(), @Options(), @Head(), @All(),

<br>

## ๐ Route wildcards

<br>

๐ ํจํด ๊ธฐ๋ฐ์ ๊ฒฝ๋ก ์ญ์ ์ ๊ณต๋๋ค. ์๋ฅผ ๋ค์ด asterisk(\*)๋ ๋ชจ๋  ๋ฌธ์์ด๊ณผ ์ผ์นํ๋ค.

<br>

```jsx
@Get('ab*cd')
findAll() {
  return 'This route uses a wildcard';
}
```

<br>

`'ab*cd'` ๋ ย `abcd`,ย `ab_cd`,ย `abecd` ์ ๋งค์นญ๋๋ค. ๊ทธ ์ธ์ `?`,ย `+`,ย `*`, ย `()` ์ญ์ ์ฌ์ฉ ๊ฐ๋ฅํ๋ค.

<br>

## ๐ ์ํ ์ฝ๋

<br>

๐ ์์์ ๋ค๋ค๋ฏ์ด, Post ์์ฒญ์ ์ ์ธํ ์ํ ์ฝ๋๋ ํญ์ 200์ด๋ค. ์ฐ๋ฆฌ๋ ์ฝ๊ฒ Hendler level์์ `@HttpCode(โฆ)` ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ด์ฉํ์ฌ ์ํ ์ฝ๋๋ฅผ ๋ณํ์ํฌ ์ ์๋ค.

<br>

```jsx
@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```

<br>

> HttpCode๋ `@nextjs/common` package๋ก๋ถํฐ import ๋๋ค.

<br>

์ข์ข ์ํ์ฝ๋๋ ๋ค์ํ ์์์ ์ํด ๋ฌ๋ผ์ง ์ ์๋ค. ์ด๋ฌํ ๊ฒฝ์ฐ library-specific response object๋ฅผ ์ด์ฉํ์ฌ ํด๊ฒฐํ  ์ ์๋ค.
โ error ์ํฉ, throw an exception ๋ฑ๋ฑ

<br>

## ๐ Headers

<br>

๐ ์ปค์คํ ์๋ต ํค๋๋ฅผ ์ง์ ํ๋ ค๋ฉด `@Header()` ๋ฐ์ฝ๋ ์ดํฐ ๋๋ library-specific response object(res.header())๋ฅผ ์ด์ฉํ๋ฉด ๋๋ค.

<br>

```jsx
@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```

<br>

> Header๋ `@nextjs/common` package๋ก๋ถํฐ import ๋๋ค.

<br>

## ๐ Redirection

<br>

๐ ํน์  URL๋ก redirect๋ฅผ ํ๊ณ ์ถ๋ค๋ฉด `@Redirect()` ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ฐ๊ฑฐ๋ library-specific response object(res.redirect())๋ฅผ ์ด์ฉํ๋ฉด ๋๋ค.

`@Redirect()` ๋ url๊ณผ statusCode ๋ ๊ฐ์ง์ ์ธ์๋ฅผ ๋ฐ๋๋ฐ, ๋ ๋ค optional์ด๋ค. ์ด ๋์ ๊ธฐ๋ณธ๊ฐ์ 302์ด๋ค.

<br>

```jsx
@Get()
@Redirect('https://nestjs.com', 301)
```

<br>

๋๋๋ก ๋์ ์ผ๋ก ๊ฒฐ์ ๋์ด์ผ ํ  ๋๊ฐ ์๋ค. ์ด๋ด ๊ฒฝ์ฐ return ๊ฐ์ ์ด์ฉํ์ฌ @Redirect()๋ฅผ override ํ  ์ ์๋ค.

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
