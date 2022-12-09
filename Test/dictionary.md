<br>

# 📌  용어 정리

<br>

- Test Double
  - 테스트를 목적으로 대역을 쓰는 것이 Test Double이다.
  - “원본 객체야 물러나 있어. 테스트는 내가 대신 해줄게!”

<br>

- Mock
  - 스마트폰 전시장을 가면 전시되어 있는 핸드폰이라 생각하면 된다.
  - 가짜다. 실제와 동일한 기능을 하지는 않지만 대충 이런 기능이 이렇게 동작할 것이라고 알려주는 용도이다.
  - 테스트에서는 호출시 동작이 잘 되었는지를 확인하는데 쓰인다.

<br>

- Stub
  - Stub이라는 단어가 내포하는 바가 전체 중 일부라는 뜻이다.
  - 모든 기능 대신 일부 기능에 집중하여 임의로 구현한다.
  - 일부 기능이라 함은 테스트를 하고자 하는 기능을 의미한다.

<br>

# 📌  예시

<br>

참고 - [typescript로 배우는 stub, mock, spy의 차이점](https://charming-kyu.tistory.com/41)

<br>

```tsx
class MailService{
	send(message: string): void {
		// 메일 전송 로직
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
    this.mailer.send(`상품 이름:${this.name} 상품 가격:${this.price}`);

}
```

<br>

👉  검증해야할 위의 코드가 존재한다고 했을 때, Stub기반과 Mock 기반의 코드를 어떻게 짜야하는지에 대해 알아보자.

먼저 Stub 기반 테스트 코드를 살펴보자.

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
	it('confirm 했을 때 메일을 발송한다.', function(){

		const order = new Order('name', 15000);
		const mailService = new MailServiceStub();
		order.setMailer(mailService);

		order.confirm();

		expect(mailService.numberSent()).toBe(1);
	};
});
```

<br>

Mock 기반 테스트 코드를 살펴보자.

<br>

```tsx
describe('Order (mock)', function () {
  beforeEach(() => jest.clearAllMocks());

  it('confirm 했을 때 메일을 발송한다.', function () {
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

Stub 기반 코드는 상태 검증(최종 결과)이고, Mock 기반 테스트는 행동 검증(올바른 호출이다.

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ccbd7157-e2d3-4e93-9d11-bf56d102cb31/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221209%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221209T064451Z&X-Amz-Expires=86400&X-Amz-Signature=9cd64f0576a50243bdf6a987f56f067830856f6b9771abf1fab15b301a2e2d2c&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

- 컨트롤러를 테스트 한다고 했을 때, service 객체는 Mock으로 만들어서 이게 호출되었는지 확인…?

<br>

- repository에서는 stub을 만들어서 상태값이 잘 저장되는지 확인…?

<br>
