<br>

# ๐ย  ์ฉ์ด ์ ๋ฆฌ

<br>

- Test Double
  - ํ์คํธ๋ฅผ ๋ชฉ์ ์ผ๋ก ๋์ญ์ ์ฐ๋ ๊ฒ์ด Test Double์ด๋ค.
  - โ์๋ณธ ๊ฐ์ฒด์ผ ๋ฌผ๋ฌ๋ ์์ด. ํ์คํธ๋ ๋ด๊ฐ ๋์  ํด์ค๊ฒ!โ

<br>

- Mock
  - ์ค๋งํธํฐ ์ ์์ฅ์ ๊ฐ๋ฉด ์ ์๋์ด ์๋ ํธ๋ํฐ์ด๋ผ ์๊ฐํ๋ฉด ๋๋ค.
  - ๊ฐ์ง๋ค. ์ค์ ์ ๋์ผํ ๊ธฐ๋ฅ์ ํ์ง๋ ์์ง๋ง ๋์ถฉ ์ด๋ฐ ๊ธฐ๋ฅ์ด ์ด๋ ๊ฒ ๋์ํ  ๊ฒ์ด๋ผ๊ณ  ์๋ ค์ฃผ๋ ์ฉ๋์ด๋ค.
  - ํ์คํธ์์๋ ํธ์ถ์ ๋์์ด ์ ๋์๋์ง๋ฅผ ํ์ธํ๋๋ฐ ์ฐ์ธ๋ค.

<br>

- Stub
  - Stub์ด๋ผ๋ ๋จ์ด๊ฐ ๋ดํฌํ๋ ๋ฐ๊ฐ ์ ์ฒด ์ค ์ผ๋ถ๋ผ๋ ๋ป์ด๋ค.
  - ๋ชจ๋  ๊ธฐ๋ฅ ๋์  ์ผ๋ถ ๊ธฐ๋ฅ์ ์ง์คํ์ฌ ์์๋ก ๊ตฌํํ๋ค.
  - ์ผ๋ถ ๊ธฐ๋ฅ์ด๋ผ ํจ์ ํ์คํธ๋ฅผ ํ๊ณ ์ ํ๋ ๊ธฐ๋ฅ์ ์๋ฏธํ๋ค.

<br>

# ๐ย  ์์

<br>

์ฐธ๊ณ  - [typescript๋ก ๋ฐฐ์ฐ๋ stub, mock, spy์ ์ฐจ์ด์ ](https://charming-kyu.tistory.com/41)

<br>

```tsx
class MailService{
	send(message: string): void {
		// ๋ฉ์ผ ์ ์ก ๋ก์ง
		console.log(message);
	}
}

class Order {
  private readonly name: string;
  private readonly price: number;
  mailer: MailService;

  constructor(name: string, price: number) {
    this.name = name;
    this.price = price;
  }

  setMailer(mailer: MailService) {
    this.mailer = mailer;
  }

  confirm() {
    this.mailer.send(`์ํ ์ด๋ฆ:${this.name} ์ํ ๊ฐ๊ฒฉ:${this.price}`);

}
```

<br>

๐ย  ๊ฒ์ฆํด์ผํ  ์์ ์ฝ๋๊ฐ ์กด์ฌํ๋ค๊ณ  ํ์ ๋, Stub๊ธฐ๋ฐ๊ณผ Mock ๊ธฐ๋ฐ์ ์ฝ๋๋ฅผ ์ด๋ป๊ฒ ์ง์ผํ๋์ง์ ๋ํด ์์๋ณด์.

๋จผ์  Stub ๊ธฐ๋ฐ ํ์คํธ ์ฝ๋๋ฅผ ์ดํด๋ณด์.

<br>

```tsx
class MailServiceStub extends MailService {
  messages: Array<string>;

  constructor() {
    super();
    this.messages = new Array<string>();
  }

  override send(message: string) {
    this.messages.push(message);
  }

  numberSent(): number {
    return this.messages.length;
  }
}
```

<br>

```tsx
describe('Order (stub)', function(){
	it('confirm ํ์ ๋ ๋ฉ์ผ์ ๋ฐ์กํ๋ค.', function(){

		const order = new Order('name', 15000);
		const mailService = new MailServiceStub();
		order.setMailer(mailService);

		order.confirm();

		expect(mailService.numberSent()).toBe(1);
	};
});
```

<br>

Mock ๊ธฐ๋ฐ ํ์คํธ ์ฝ๋๋ฅผ ์ดํด๋ณด์.

<br>

```tsx
describe('Order (mock)', function () {
  beforeEach(() => jest.clearAllMocks());

  it('confirm ํ์ ๋ ๋ฉ์ผ์ ๋ฐ์กํ๋ค.', function () {
    // given
    const mailService = new MailService();
    mailService.send = jest.fn();
    const mailServiceSend = mailService.send;
    const order = new Order('name', 15000);
    order.setMailer(mailService);

    // when
    order.confirm();

    // then
    expect(mailServiceSend).toBeCalledTimes(1);
  });
});
```

<br>

Stub ๊ธฐ๋ฐ ์ฝ๋๋ ์ํ ๊ฒ์ฆ(์ต์ข ๊ฒฐ๊ณผ)์ด๊ณ , Mock ๊ธฐ๋ฐ ํ์คํธ๋ ํ๋ ๊ฒ์ฆ(์ฌ๋ฐ๋ฅธ ํธ์ถ์ด๋ค.

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ccbd7157-e2d3-4e93-9d11-bf56d102cb31/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221209%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221209T064451Z&X-Amz-Expires=86400&X-Amz-Signature=9cd64f0576a50243bdf6a987f56f067830856f6b9771abf1fab15b301a2e2d2c&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

- ์ปจํธ๋กค๋ฌ๋ฅผ ํ์คํธ ํ๋ค๊ณ  ํ์ ๋, service ๊ฐ์ฒด๋ Mock์ผ๋ก ๋ง๋ค์ด์ ์ด๊ฒ ํธ์ถ๋์๋์ง ํ์ธโฆ?

<br>

- repository์์๋ stub์ ๋ง๋ค์ด์ ์ํ๊ฐ์ด ์ ์ ์ฅ๋๋์ง ํ์ธโฆ?

<br>
