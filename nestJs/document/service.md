<br>

# ๐ย  NestJS ๊ณต์๋ฌธ์ Service ๊ณต๋ถํ๊ธฐ

<br>

## ๐ Providers

<br>

๐ Nest classes๋ services, repositories, factories, helpers ๋ฑ์ ๊ณต๊ธ์๋ก ์ทจ๊ธํ  ์ ์๋ค.
์ด๋ฌํ provider๋ ์์กด์ฑ์ ์ฃผ์ํ  ์ ์๋ค. ์ด๊ฒ์ด ์๋ฏธํ๋ ๊ฒ์ ๋ค์ํ ๊ด๊ณ๋ฅผ ๋ง๋ค ์ ์๋ค๋ ๊ฒ์ด๋ค. ์ด๋ฌํ ๊ธฐ๋ฅ์ Nest์ ๋ฐํ์ ์์คํ์ ์์ ๋ฐ์ ์ ์๋คโฆ?

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2b488ca9-f012-45c1-94dd-da385f59c0af/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221124T072135Z&X-Amz-Expires=86400&X-Amz-Signature=970e9a5fa2d32eb1ec3d0b0f8d422fddbf58ed1a33f554a1f668bc836f631363&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

์ฐ๋ฆฌ๋ ์ด์  ์ฑํฐ์์ CatsController๋ฅผ ๋ง๋ค์๋ค. Controllers๋ HTTP requests๋ฅผ ๋ค๋ค์ผ ํ๊ณ  providers์ ๋ ๋ณต์กํ ์ผ๋ค์ ์์ํด์ผ ํ๋ค. Providers๋ ๋ชจ๋์์ ๊ณต๊ธ์๋ก ์ ์ธ๋๋ ์ผ๋ฐ JS classes์ด๋ค.

์ฆ, ์ปจํธ๋กค๋ฌ๋ ์์ ์ด ํ  ์ผ์๋ง ์ง์คํ๊ณ  ๋ ๋ณต์กํ ๊ฒ๋ค์ ๋ํด์  ์ฑ์์ ์ง์ง ์๊ณ , Providers๊ฐ ์ฒ๋ฆฌํ  ์ ์๋๋ก ์์ํด์ผ ํ๋ค.

<br>

> Nest๋ SOLID ์์น์ ๋ฐ๋ฅด๋ ๊ฒ์ด ์ข๋ค.

<br>

## ๐ Services

<br>

๐ ์ ์ด์  CastsService๋ฅผ ๋ง๋ค ๊ฒ์ด๋ค. ์ด ์๋น์ค๋ ๋ฐ์ดํฐ์ ์ ์ฅ๊ณผ ๊ฒ์์ ํ  ์ฑ์์ด ์๊ณ , CatsController์์ ์ฌ์ฉ๋  ์ ์๋๋ก ์ค๊ณ๋์๋ค. ๋ฐ๋ผ์ ๊ณต๊ธ์๋ก ์ ์ํ๊ธฐ์ ์ข๋ค.

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

> CLI๋ฅผ ์ฌ์ฉํ์ฌ ์๋น์ค๋ฅผ ์์ฑํ๋ ค๋ฉด $ nest g service cats ๋ช๋ น์ ์คํํ๋ฉด ๋๋ค.

<br>

๋์ฌ๊ฒจ๋ณผ ์ ์ @Injectable() ๋ฐ์ฝ๋ ์ดํฐ์ด๋ค. ์ด๋ ๋ฉํ๋ฐ์ดํฐ๋ฅผ ์ฒจ๋ถํ๋ค. ์ด๊ฒ์ด ๋ปํ๋ ๊ฑด CatsService๋ Nest IoC ์ปจํ์ด๋์์ ๊ด๋ฆฌํ  ์ ์๋ ํด๋์ค๋ผ๋ ๊ฒ์ด๋ค.
๊ทธ๊ฑด ๊ทธ๋ ๊ณ  ์ด ์์ ๋ ๋ค์๊ณผ ๊ฐ์ Cat ์ธํฐํ์ด์ค๋ ์ฌ์ฉํ๋ค.

 <br>

```jsx
export interface Cat {
  name: string;
  age: number;
  breed: string;
}
```

<br>

์ด์  ๊ณ ์์ด๋ฅผ ๊ฒ์ํ๋ ์๋น์ค ํด๋์ค๊ฐ ์์ผ๋ฏ๋ก CatsController ๋ด์์ ์ฌ์ฉํ๊ฒ ๋ค.

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

CatsService๋ ํด๋์ค์ ์์ฑ์๋ฅผ ํตํด ์ฃผ์๋์๋ค. ๊ทธ๋ฆฌ๊ณ  ์ด๋ private syntax๊ฐ ๋ถ์ด์๋ ๊ฑธ ์ฃผ๋ชฉํ์. ์ด๋ฅผ ์ฌ์ฉํ๋ฉด ๋์ผํ ์์น์์ catsService ๋ฉค๋ฒ๋ฅผ ์ฆ์ ์ ์ธํ๊ณ  ์ด๊ธฐํํ  ์ ์๋ค.

์ฆ, Nest์์ ์์์ ์ ์ธ๊ณผ ์ด๊ธฐํ๋ฅผ ํด์ฃผ๋๊น ๊ทธ๋ฅ ๋ฐ๋ก ์ฌ์ฉํ๋ฉด ๋๋ค๋ ๋ป!

<br>

## ๐ Dependency injection

<br>

๐ Nest๋ DI๋ผ๊ณ  ์๋ ค์ง ๊ฐ๋ ฅํ ๋์์ธ ํจํด์ ๊ธฐ๋ฐ์ผ๋ก ๊ตฌ์ถ๋์๋ค.

Nest์์๋ TS์ ๊ธฐ๋ฅ๋๋ถ์ ์ข์์ฑ์ด ์ ํ๋ณ๋ก ํด๊ฒฐ๋๊ธฐ ๋๋ฌธ์ ๋งค์ฐ ์ฝ๊ฒ ์ข์์ฑ์ ๊ด๋ฆฌํ  ์ ์๋ค. ์๋ฅผ ๋ค๋ฉด Nest๋ CatsService ์ธ์คํด์ค๋ฅผ ์์ฑํ๊ณ  ๋ฐํํ์ฌ catsService๋ฅผ ํด๊ฒฐํ๋ค. ์ด ์ข์์ฑ์ ํด๊ฒฐ๋์ด ์ปจํธ๋กค๋ฌ์ ์์ฑ์๋ก ์ ๋ฌ๋๋ค.

<br>

```jsx
constructor(private catsService: CatsService) {}
```

<br>

์์ง ์ ์ดํด๊ฐ ๋์ง ์๋๋ค. ํ์ง๋ง ์ผ๋จ Module์ Providers ๋ฐฐ์ด ์์ ๋ค์ด๊ฐ Providers๋ Nest๊ฐ ์ธ์คํด์ค๋ฅผ ์์ฑํ๊ณ  ๋ฐํํ์ฌ ๊ทธ ์ธ์คํด์ค๋ฅผ ์ปจํธ๋กค๋ฌ์ ์์ฑ์๋ก ์ ๋ฌํ๋ค๊ณ  ์ดํดํ๋ค.

<br>

## ๐ Scopes

<br>

๐ ๊ณต๊ธ์๋ ์ ํ๋ฆฌ์ผ์ด์์ ์๋ช ์ฃผ๊ธฐ์ ๋๊ธฐํ๋ ์๋ช์ฃผ๊ธฐ(โ๋ฒ์โ)๋ฅผ ๊ฐ๋๋ค.
์ ํ๋ฆฌ์ผ์ด์์ด ๋ถํธ์คํธ๋ฉ๋๋ฉด ๋ชจ๋  ์ข์์ฑ์ ํด๊ฒฐํด์ผ ํ๋ฏ๋ก ๋ชจ๋  Providers๋ฅผ ์ธ์คํด์คํ ํด์ผํ๋ค. ๋ง์ฐฌ๊ฐ์ง๋ก ์ ํ๋ฆฌ์ผ์ด์์ด ์ข๋ฃ๋๋ฉด ๊ฐ Providers๋ ์๋ฉธ๋๋ค.

๊ทธ๋ฌ๋ ๋ฐ๋ก Providers์ ์๋ช์ฃผ๊ธฐ๋ฅผ ์กฐ์ํ  ์ ์๋ ๋ฐฉ๋ฒ๋ ์๋ค. ์ด๋ ๋์ค์ ์์๋ณด์!!

<br>

## ๐ Custom providers

<br>

๐ Nest์๋ Providers์ ๊ด๊ณ๋ฅผ ํด๊ฒฐํ๋ IoC ์ปจํ์ด๋๊ฐ ๋ด์ฅ๋์ด ์๋ค. ์ด๋ ์์์ ์ค๋ชํ DI์ ๊ธฐ๋ฐ์ด ๋์ง๋ง ์ง๊ธ๊น์ง์ ์ค๋ช๋ณด๋ค ํจ์ฌ ๊ฐ๋ ฅํ๋ค. Providers๋ฅผ ์ ์ํ๋ ๋ฐฉ๋ฒ์๋ ์ฌ๋ฌ๊ฐ์ง๊ฐ ์๋ค.

<br>

๐ Nest์๋ Providers์ ๊ด๊ณ๋ฅผ ํด๊ฒฐํ๋ IoC ์ปจํ์ด๋๊ฐ ๋ด์ฅ๋์ด ์๋ค. ์ด๋ ์์์ ์ค๋ชํ DI์ ๊ธฐ๋ฐ์ด ๋์ง๋ง ์ง๊ธ๊น์ง์ ์ค๋ช๋ณด๋ค ํจ์ฌ ๊ฐ๋ ฅํ๋ค. Providers๋ฅผ ์ ์ํ๋ ๋ฐฉ๋ฒ์๋ ์ฌ๋ฌ๊ฐ์ง๊ฐ ์๋ค.
โ ์ผ๋ฐ ๊ฐ, ํด๋์ค ๋ฐ ๋น๋๊ธฐ ๋๋ ๋๊ธฐ ํฉํ ๋ฆฌ

<br>

์ด๊ฒ๋ ์ถํ์ ์์๋ณด๋๋ก ํ์!!

<br>

## ๐ Optional providers

<br>

๐ ๊ฒฝ์ฐ์ ๋ฐ๋ผ์ ํด๊ฒฐํ  ํ์๊ฐ ์๋ ์ข์์ฑ์ด ์์ ์ ์๋ค. ์ด๋ฌํ ๊ฒฝ์ฐ Provider๊ฐ ์์ด๋ ์ค๋ฅ๊ฐ ๋ฐ์ํ์ง ์์ผ๋ฏ๋ก ์ข์์ฑ์ด ์ ํ ์ฌํญ์ด ๋๋ค.

Provider๊ฐ ์ ํ ์ฌํญ์ผ์ ๋ํ๋ด๋ ค๋ฉด `@Optional()` ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์์ฑ์โs signature์ ๋ฃ์ด์ฃผ๋ฉด ๋๋ค.

<br>

```jsx
import { Injectable, Optional, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  constructor(@Optional() @Inject('HTTP_OPTIONS') private httpClient: T) {}
}
```

<br>

Custom provider๋ฅผ ์ฌ์ฉํ๊ณ  ์๊ธฐ ๋๋ฌธ์ โHTTP_OPTIONSโ๋ผ๋ Custom token์ ์ฌ์ฉํ๊ณ  ์๋ค.

์ด ๋ถ๋ถ ์ญ์ ๋์ค์ ๋ ์์ธํ ์์๋ณด๋๋ก ํ์!

<br>

## ๐ Provider registration

<br>

๐ ์ด์  provider(CatsService)๋ฅผ ์ ์ํ๊ณ , ํด๋น ์๋น์ค์ ์๋น์(CatsController)๊ฐ ์์ผ๋ฏ๋ก Injection์ ์ํํ๊ธฐ ์ํด service๋ฅผ ๋ฑ๋กํ  ํ์๊ฐ ์๋ค.
์ด๋ฅผ ์ํด์๋ module file(app.module.ts)์์ `@Module()` ๋ฐ์ฝ๋ ์ดํฐ์ providers ๋ฐฐ์ด์ ์๋น์ค๋ฅผ ๋ํด์ผํ๋ค.

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

Nest๋ ์ด์  CatsController ํด๋์ค์ ์ข์์ฑ์ ํด๊ฒฐํ  ์ ์๋ค.

์ด์  ๋๋ ํ ๋ฆฌ์ ๊ตฌ์กฐ๋ ๋ค์๊ณผ ๊ฐ๋ค.

<br>

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3d9d3348-9956-4580-9cf2-369d7614222f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221124T072155Z&X-Amz-Expires=86400&X-Amz-Signature=6e893b46e11342b7a418f6436350a9b7a9e26e052d88310ed7473539dd55e851&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)
