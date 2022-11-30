<br>

## 🔎  Passport-jwt를 이용하여 jwt 토큰을 발급해보자!

<br>

```tsx
// cats.controller.ts

export class CatsController {

	...

	@ApiOperation({ summary: '로그인' })
  @Post('login')
  logIn(@Body() data: LoginRequestDto) {
    return this.authService.jwtLogin(data);
  }

	...
}
```

<br>

👉  로그인을 위해 `/cats/login` 로 요청을 보낸다. 로그인 처리를 위해 authService.jwtLogin() 메서드를 호출한다.

<br>

```tsx
// auth.service.ts

@Injectable()
export class AuthService {
  constructor(
    private readonly catsRepository: CatsRepository,
    private jwtService: JwtService // JwtService는 @nestjs/jwt에서 제공해주는 Built-in된 service이다.
  ) {}

  async jwtLogin(data: LoginRequestDto) {
    const { email, password } = data;

    const cat = await this.catsRepository.findCatByEmail(email);

    if (!cat) {
      throw new UnauthorizedException('이메일과 비밀번호를 확인해주세요.');
    }

    const isPasswordValidated: boolean = await bcrypt.compare(
      password,
      cat.password
    );

    if (!isPasswordValidated) {
      throw new UnauthorizedException('이메일과 비밀번호를 확인해주세요.');
    }

    const payload = { email: email, sub: cat.id };

    return {
      token: this.jwtService.sign(payload),
    };
  }
}
```

<br>

이메일과 비밀번호를 검증하여 로그인 처리를 한 후 jwtService의 sign() 메서드를 통해 토큰을 return해준다. 이 return된 토큰은 클라이언트로 전달이 된다.

이제 Nest App에서 위에서 정의한 모듈들을 사용할 수 있게 하기 위해 auth.module.ts 파일을 구성하러 가보자!

<br>

```tsx
// auth.auth.module.ts

@Module({
  imports: [
    forwardRef(() => CatsModule),
    PassportModule,
    JwtModule.register({
      secret: key 값 // JWT sign과 verify를 할 때 사용한다.
      signOptions: { expiresIn: '60s' }, // token의 유효 시간을 설정해준다.
    }),
  ],
  providers: [AuthService],
  exports: [AuthService],
})
export class AuthModule {}
```

<br>

auth에서 사용할 모듈들을 imports 시켜줬다.

그 결과 성공적으로 로그인에 성공 후 토큰을 발급 받는 모습을 볼 수 있다.

<br>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f7181e12-fcc9-4b1a-9461-087ce6f2e1f3/Untitled.png)

<br>

## 🔎  JWT 토큰을 이용하여 인증 처리

<br>

👉  자 이제 토큰을 발급해줬으니 이를 어떻게 활용하는지 살펴보자.

서버에서 발급해준 토큰은 클라이언트가 어딘가에 보관을 한다. 그 후 다른 요청을 보낼 때 토큰을 첨부한다. 서버는 이를 검증한 후 접근을 인정해줄지 아닐지 결정을 한다.

먼저 검증을 위한 jwt 전략을 구현하자!

<br>

```tsx
// auth/jwt.strategy.ts

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      // 요청에서 JWT를 추출하는 방법을 제공해준다.
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),

      // 기한이 만기된 토큰이 오면 요청이 거부되고 401 에러를 던질 것인지
      ignoreExpiration: false,

      // 토큰 확인을 위한 대칭키이다. 이때 값은 같아야 한다!
      secretOrKey: jwtConstants.secret,
    });
  }

  async validate(payload: Payload) {
    const cat = await this.catsRepository.findCatByIdWithoutPassword(
      payload.sub
    );

    if (cat) {
      return cat; // request.user
    } else {
      throw new UnauthorizedException('접근 오류');
    }
  }
}
```

<br>

validate() 메서드 안에서 우리는 다양한 인증 처리를 할 수 있다. 그 후 passport는 validate()의 return 값으로 user를 만들고 Request 객체에 첨부한다.

위의 jwt 전략을 사용하기 위해선 authModule에 공급자로 넣어줘야 한다.

<br>

```tsx
// auth.auth.module.ts

@Module({
  imports: [
    forwardRef(() => CatsModule),
    PassportModule,
    JwtModule.register({
      secret: key 값 // JWT sign과 verify를 할 때 사용한다.
      signOptions: { expiresIn: '60s' }, // token의 유효 시간을 설정해준다.
    }),
  ],
  providers: [AuthService, JwtStrategy],
  exports: [AuthService],
})
export class AuthModule {}
```

<br>

자 이제 컨트롤러에서 Guard를 사용함으로써 JWT 인증을 사용할 수 있다.

<br>

```tsx
 // cats.controller.ts

export class CatsController {

	...

	@ApiOperation({ summary: '프로필 가져오기' })
	@UseGuards(AuthGuard('jwt'))
  @Get('profile')
	getProfile(@Req() req){
		return req.user;
	}
	...
}
```
