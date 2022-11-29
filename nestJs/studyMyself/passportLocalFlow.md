<br>

# 📌  Passport-Local 코드의 흐름

<br>

## 🔎  요청이 들어오고, 미들웨어를 거친다.

<br>

👉  말 그대로 Request LifeCycle에 의해 요청이 들어온 후, Nest App에 작성된 미들웨어들이 실행된다.

<br>

## 🔎   Global → Controller → Route 순서대로 Guards가 실행된다.

<br>

👉  오늘 공부한 local은 Route에만 Guards가 있었기 때문에 Route Guards가 실행되었다.

<br>

```tsx
// app.controller.ts

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

 ...

  @UseGuards(AuthGuard('local'))
  @Post('auth/login')
  async login(@Request() req) {
    return req.user;
  }
}
```

<br>

@nestjs/passport에서 제공해주는 Built-in된 AuthGuard가 실행이 된다.

<br>

## 🔎   LocalStrategy의 validate()가 호출된다.

<br>

👉  이때 signiture에 ‘local’이 있기 때문에, local과 매칭되는 LocalStrategy의 validate()가 호출된다.

<br>

```tsx
// local.strategy.ts

@Injectable()
export class LocalStrategy extends PassportStrategy(Strategy) {
  constructor(private authService: AuthService) {
    super();
  }

  async validate(username: string, password: string): Promise<any> {
    console.log('here is LocalStrategy');
    const user = await this.authService.validateUser(username, password);

    console.log('곧 에러다잇');

    if (!user) {
      throw new UnauthorizedException();
    }

    return user;
  }
}
```

<br>

validate()는 passport-local 전략을 따르기 때문에 username과 password를 받아오고, 이를 이용하여 검증을 한다.

validate()의 return 값에 따라 route handler 함수가 실행될 수도, 에러가 발생할 수도 있다.

<br>

## 🔎   authService의 validateUser()가 호출된다.

<br>

👉  공식문서에서는 authService.validateUser()를 호출했기에 실행이 된다.

<br>

```tsx
// auth.service.ts

@Injectable()
export class AuthService {
  constructor(private usersService: UsersService) {}

  async validateUser(username: string, pass: string): Promise<any> {
    console.log('here is AuthService');
    const user = await this.usersService.findOne(username);

    if (user && user.password === pass) {
      const { password, ...result } = user;
      password;
      return result;
    }

    console.log('return null ㅠㅠ');

    return null;
  }
}
```

<br>

validateUser()는 DB에서 유저 정보를 가져와서 검증을 수행한다.
이때, 값이 있다면 return 해주고 없다면 null을 return 해준다.

<br>

## 🔎   UserService의 findOne()이 호출된다.

<br>

```tsx
// users.service.ts

@Injectable()
export class UsersService {
  private readonly users = [
    {
      userId: 1,
      username: 'john',
      password: 'changeme',
    },
    {
      userId: 2,
      username: 'maria',
      password: 'guess',
    },
  ];

  async findOne(username: string): Promise<User | undefined> {
    return this.users.find((user) => user.username === username);
  }
}
```

<br>

👉  예제에서는 Array를 사용했지만, 실제 프로젝트에서는 Mongoose 등을 이용하여 DB에서 데이터를 가져왔을 것이다.

<br>

## 🔎   예외 처리 파이프 or Route handler

<br>

👉  인증 결과에 따라 예외 처리 파이프로 가거나 Route handler로 간다.

<br>
