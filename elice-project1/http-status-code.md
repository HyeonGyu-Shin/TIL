<br>

# 📌 &nbsp; HTTP Status Code에 대해 알아보자!

<br>

👉 &nbsp; 프로젝트 진행 중 에러 발생시 에러 코드를 프론트로 넘겨주는 부분에 대한 피드백을 받았다.

&nbsp; 피드백 내용은 상황에 맞는 상태 코드를 작성하는게 좋다였다.
확실히 어떠한 것때문에 에러가 났는지에 대해 명확하게 보내줘야 협업을 할 때 소통이 더 잘 될 수 있겠다는 생각이 들었다.

&nbsp;이에 HTTP Status Code에 대해 더 공부해보고 프로젝트에 적용해보는 시간을 가지려고 한다.

&nbsp; 자 그럼 시작해보자!

<br>

# 📌 &nbsp; What is HTTP Status Code...??

<br>

👉 &nbsp; mdn의 정의를 살펴보면 Http Status code는 HTTP request가 성공적으로 완료되었는지 여부를 나타내는 코드라고 나와있다.

&nbsp; 상태 코드는 다음의 5개의 그룹을 가지고 있다.

1. Information responses(100 ~ 199)

2. Successfult responses(200 ~ 299)

3. Redirection messages(300 ~ 399)

4. Client error responses(400 ~ 499)

5. Server error responses(500 ~ 599)

&nbsp;그 중 이번 시간에는 4, 5번에 해당하는 Client와 Server 에러에 대해 배워보도록 하겠다.

<br>

## 🔎 &nbsp; 1. Client error responses

<br>

<br>

### ✏️ &nbsp; 400 Bad Request

<br>

👉 &nbsp; 클라이언트에서 클라이언트 오류로 생각되는 요청을 보내서 서버에서 요청을 처리할 수 없을 때 사용되는 상태코드이다.
<br>

### ✏️ &nbsp; 401 Unauthorized

<br>

👉 &nbsp; 상태 코드의 이름은 Unauthorized이지만, 실제론 unauthenticated를 의미한다. 즉, 클라이언트가 인증을 받지 않았기 때문에 인증이 필요하다는 걸 나타내는 상태코드이다.

<br>

### ✏️ &nbsp; 403 Forbidden

<br>

👉 &nbsp; 클라이언트가 권한을 인가받지 않았기에 서버에서 요청을 처리할 수 없을 때 사용되는 상태코드이다.

<br>

### ✏️ &nbsp; 404 Not Found

<br>

👉 &nbsp; 브라우저와 서버, API에서 의미하는 바가 다 다르다.

1. 서버 - 요청받은 리소스를 찾을 수 없을 때 사용

2. 브라우저 - 알려지지 않은 URL을 의미

3. API - 엔드포인트는 유효하지만 리소스 자체가 존재하지 않음을 의미

<br>

## 🔎 &nbsp; 2. Server error responses

<br>

<br>

### ✏️ &nbsp; 500 Internal Server Error

<br>

👉 &nbsp; 서버가 처리를 못하는 상황일 때 사용하는 상태코드이다.

&nbsp; 서버의 상태값을 외부로 노출하는 것은 좋은 것이 아니다. 따라서 서버에서 오류가 발생할 땐 500 에러를 대부분 사용한다.

<br>

# 📌 &nbsp; 프로젝트에 이를 적용해보자!

<br>

👉 &nbsp; 먼저 OrderService에서 모든 주문을 가져오는 상황일때의 에러처리이다.

```js
// OrderService.js

class OrderService{
    ...

    async findAllOrders(){
        const allOrders = await this.orderModel.findAll();

        if(allOrdders.length < 1) return '주문 내역이 존재하지 않습니다.';

        return allOrders;
    }
}

---

 async findAllOrders(){
        const allOrders = await this.orderModel.findAll();

        if(allOrdders.length < 1) return {};

        return allOrders;
    }

```

&nbsp; 위의 코드를 아래의 코드로 변경했다.

&nbsp; 모든 데이터를 받아오는 요청에 있어서 DB에 데이터가 없어서 가져온 데이터가 없을 경우 빈 객체를 반환하면 된다.

&nbsp; 서버는 DB에 있는 데이터를 가져오는 요청을 완수했기에 에러를 던지지 않고 빈 객체만 던지면 된다. 그것에 대한 처리는 프론트에서 하면 된다.

👉 &nbsp; 다음은 특정 주문을 가져올 때의 에러처리이다.

```js
// OrderService.js

class OrderService{
    ...

    async findOrder(orderId) {
        const foundOrder = await this.orderModel.find(orderId);

        if (!foundOrder) throw new BadRequestError('해당 주문을 찾을 수 없습니다.');

        return foundOrder;
    }
}

---

  async findOrder(orderId) {
        const foundOrder = await this.orderModel.find(orderId);

        if (!foundOrder) throw new NotFoundError('해당 주문을 찾을 수 없습니다.');

         return foundOrder;
    }

```

&nbsp; orderId에 대한 유효성 검사는 컨트롤러에서 완료했다.
이에 클라이언트에서 형식에 맞게 요청은 잘 보냈다는게 결정됐다.

&nbsp; 그럼 이 아이디를 이용하여 DB에서 주문을 찾으려고 하는데, 이때 DB에 값이 없다면 이는 클라이언트가 유효하지 않는 값을 보냈거나 서버 에러이거나 둘 중 하나이다.

&nbsp; 하지만 위의 상황에서는 클라이언트가 유효하지 않은 값을 보냈기에 404 에러를 보내준다.

👉 &nbsp; 다음은 주문 생성시의 에러처리이다.

```js
// OrderService.js

class OrderService{
    ...

    async createOrder(orderInfo) {
        const { userId } = orderInfo;
        const user = await userModel.findById(userId);
        orderInfo.user = user;

        const createdOrder = await this.orderModel.create(orderInfo);

        if (!createdOrder)
            throw new BadRequestError('등록 요청을 실행할 수 없습니다.');

        return createdOrder;
    }
}

---

    async createOrder(orderInfo) {
        const createdOrder = await this.orderModel.create(orderInfo);

        const createdOrderInDB = await this.findOrder(createdOrder._id);

        if (!createdOrderInDB) throw new Error('서버에서 처리할 수 없었습니다.');

        return createdOrder;
    }

```

&nbsp; 위의 경우 에러처리를 고치려는 과정에서 코드의 수정도 했다... 하핳..

&nbsp; user를 굳이 찾아올 필요가 없었다...ㅎㅎ

&nbsp; 에러의 경우 라우터 앞에서 입력값의 유효성과 로그인 유무를 체크해줬기 때문에 오류가 날 일은 서버에서 DB에 작성을 못 한 경우밖에 없다고 생각한다.

&nbsp; 따라서 Error Handler에서 default 값인 500으로 처리할 수 있게 했다.

<br>

# 📌 &nbsp; 정리

<br>

👉 &nbsp; HTTP Status Code에 대해 알아보고 이를 이용하여 실제 프로젝트의 코드를 리팩토링 해봤다.

&nbsp; 이를 통해 코드의 흐름을 파악하기 쉬워졌고, 만약 에러가 발생한다고 해도 빠르게 에러를 파악할 수 있겠다는 생각이 들었다.
