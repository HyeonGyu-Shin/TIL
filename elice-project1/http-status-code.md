<br>

# π &nbsp; HTTP Status Codeμ λν΄ μμλ³΄μ!

<br>

π &nbsp; νλ‘μ νΈ μ§ν μ€ μλ¬ λ°μμ μλ¬ μ½λλ₯Ό νλ‘ νΈλ‘ λκ²¨μ£Όλ λΆλΆμ λν νΌλλ°±μ λ°μλ€.

&nbsp; νΌλλ°± λ΄μ©μ μν©μ λ§λ μν μ½λλ₯Ό μμ±νλκ² μ’λ€μλ€.
νμ€ν μ΄λ ν κ²λλ¬Έμ μλ¬κ° λ¬λμ§μ λν΄ λͺννκ² λ³΄λ΄μ€μΌ νμμ ν  λ μν΅μ΄ λ μ λ  μ μκ² λ€λ μκ°μ΄ λ€μλ€.

&nbsp;μ΄μ HTTP Status Codeμ λν΄ λ κ³΅λΆν΄λ³΄κ³  νλ‘μ νΈμ μ μ©ν΄λ³΄λ μκ°μ κ°μ§λ €κ³  νλ€.

&nbsp; μ κ·ΈλΌ μμν΄λ³΄μ!

<br>

# π &nbsp; What is HTTP Status Code...??

<br>

π &nbsp; mdnμ μ μλ₯Ό μ΄ν΄λ³΄λ©΄ Http Status codeλ HTTP requestκ° μ±κ³΅μ μΌλ‘ μλ£λμλμ§ μ¬λΆλ₯Ό λνλ΄λ μ½λλΌκ³  λμμλ€.

&nbsp; μν μ½λλ λ€μμ 5κ°μ κ·Έλ£Ήμ κ°μ§κ³  μλ€.

1. Information responses(100 ~ 199)

2. Successfult responses(200 ~ 299)

3. Redirection messages(300 ~ 399)

4. Client error responses(400 ~ 499)

5. Server error responses(500 ~ 599)

&nbsp;κ·Έ μ€ μ΄λ² μκ°μλ 4, 5λ²μ ν΄λΉνλ Clientμ Server μλ¬μ λν΄ λ°°μλ³΄λλ‘ νκ² λ€.

<br>

## π &nbsp; 1. Client error responses

<br>

<br>

### βοΈ &nbsp; 400 Bad Request

<br>

π &nbsp; ν΄λΌμ΄μΈνΈμμ ν΄λΌμ΄μΈνΈ μ€λ₯λ‘ μκ°λλ μμ²­μ λ³΄λ΄μ μλ²μμ μμ²­μ μ²λ¦¬ν  μ μμ λ μ¬μ©λλ μνμ½λμ΄λ€.
<br>

### βοΈ &nbsp; 401 Unauthorized

<br>

π &nbsp; μν μ½λμ μ΄λ¦μ Unauthorizedμ΄μ§λ§, μ€μ λ‘  unauthenticatedλ₯Ό μλ―Ένλ€. μ¦, ν΄λΌμ΄μΈνΈκ° μΈμ¦μ λ°μ§ μμκΈ° λλ¬Έμ μΈμ¦μ΄ νμνλ€λ κ±Έ λνλ΄λ μνμ½λμ΄λ€.

<br>

### βοΈ &nbsp; 403 Forbidden

<br>

π &nbsp; ν΄λΌμ΄μΈνΈκ° κΆνμ μΈκ°λ°μ§ μμκΈ°μ μλ²μμ μμ²­μ μ²λ¦¬ν  μ μμ λ μ¬μ©λλ μνμ½λμ΄λ€.

<br>

### βοΈ &nbsp; 404 Not Found

<br>

π &nbsp; λΈλΌμ°μ μ μλ², APIμμ μλ―Ένλ λ°κ° λ€ λ€λ₯΄λ€.

1. μλ² - μμ²­λ°μ λ¦¬μμ€λ₯Ό μ°Ύμ μ μμ λ μ¬μ©

2. λΈλΌμ°μ  - μλ €μ§μ§ μμ URLμ μλ―Έ

3. API - μλν¬μΈνΈλ μ ν¨νμ§λ§ λ¦¬μμ€ μμ²΄κ° μ‘΄μ¬νμ§ μμμ μλ―Έ

<br>

## π &nbsp; 2. Server error responses

<br>

<br>

### βοΈ &nbsp; 500 Internal Server Error

<br>

π &nbsp; μλ²κ° μ²λ¦¬λ₯Ό λͺ»νλ μν©μΌ λ μ¬μ©νλ μνμ½λμ΄λ€.

&nbsp; μλ²μ μνκ°μ μΈλΆλ‘ λΈμΆνλ κ²μ μ’μ κ²μ΄ μλλ€. λ°λΌμ μλ²μμ μ€λ₯κ° λ°μν  λ 500 μλ¬λ₯Ό λλΆλΆ μ¬μ©νλ€.

<br>

# π &nbsp; νλ‘μ νΈμ μ΄λ₯Ό μ μ©ν΄λ³΄μ!

<br>

π &nbsp; λ¨Όμ  OrderServiceμμ λͺ¨λ  μ£Όλ¬Έμ κ°μ Έμ€λ μν©μΌλμ μλ¬μ²λ¦¬μ΄λ€.

```js
// OrderService.js

class OrderService{
    ...

    async findAllOrders(){
        const allOrders = await this.orderModel.findAll();

        if(allOrdders.length < 1) return 'μ£Όλ¬Έ λ΄μ­μ΄ μ‘΄μ¬νμ§ μμ΅λλ€.';

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

&nbsp; μμ μ½λλ₯Ό μλμ μ½λλ‘ λ³κ²½νλ€.

&nbsp; λͺ¨λ  λ°μ΄ν°λ₯Ό λ°μμ€λ μμ²­μ μμ΄μ DBμ λ°μ΄ν°κ° μμ΄μ κ°μ Έμ¨ λ°μ΄ν°κ° μμ κ²½μ° λΉ κ°μ²΄λ₯Ό λ°ννλ©΄ λλ€.

&nbsp; μλ²λ DBμ μλ λ°μ΄ν°λ₯Ό κ°μ Έμ€λ μμ²­μ μμνκΈ°μ μλ¬λ₯Ό λμ§μ§ μκ³  λΉ κ°μ²΄λ§ λμ§λ©΄ λλ€. κ·Έκ²μ λν μ²λ¦¬λ νλ‘ νΈμμ νλ©΄ λλ€.

π &nbsp; λ€μμ νΉμ  μ£Όλ¬Έμ κ°μ Έμ¬ λμ μλ¬μ²λ¦¬μ΄λ€.

```js
// OrderService.js

class OrderService{
    ...

    async findOrder(orderId) {
        const foundOrder = await this.orderModel.find(orderId);

        if (!foundOrder) throw new BadRequestError('ν΄λΉ μ£Όλ¬Έμ μ°Ύμ μ μμ΅λλ€.');

        return foundOrder;
    }
}

---

  async findOrder(orderId) {
        const foundOrder = await this.orderModel.find(orderId);

        if (!foundOrder) throw new NotFoundError('ν΄λΉ μ£Όλ¬Έμ μ°Ύμ μ μμ΅λλ€.');

         return foundOrder;
    }

```

&nbsp; orderIdμ λν μ ν¨μ± κ²μ¬λ μ»¨νΈλ‘€λ¬μμ μλ£νλ€.
μ΄μ ν΄λΌμ΄μΈνΈμμ νμμ λ§κ² μμ²­μ μ λ³΄λλ€λκ² κ²°μ λλ€.

&nbsp; κ·ΈλΌ μ΄ μμ΄λλ₯Ό μ΄μ©νμ¬ DBμμ μ£Όλ¬Έμ μ°ΎμΌλ €κ³  νλλ°, μ΄λ DBμ κ°μ΄ μλ€λ©΄ μ΄λ ν΄λΌμ΄μΈνΈκ° μ ν¨νμ§ μλ κ°μ λ³΄λκ±°λ μλ² μλ¬μ΄κ±°λ λ μ€ νλμ΄λ€.

&nbsp; νμ§λ§ μμ μν©μμλ ν΄λΌμ΄μΈνΈκ° μ ν¨νμ§ μμ κ°μ λ³΄λκΈ°μ 404 μλ¬λ₯Ό λ³΄λ΄μ€λ€.

π &nbsp; λ€μμ μ£Όλ¬Έ μμ±μμ μλ¬μ²λ¦¬μ΄λ€.

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
            throw new BadRequestError('λ±λ‘ μμ²­μ μ€νν  μ μμ΅λλ€.');

        return createdOrder;
    }
}

---

    async createOrder(orderInfo) {
        const createdOrder = await this.orderModel.create(orderInfo);

        const createdOrderInDB = await this.findOrder(createdOrder._id);

        if (!createdOrderInDB) throw new Error('μλ²μμ μ²λ¦¬ν  μ μμμ΅λλ€.');

        return createdOrder;
    }

```

&nbsp; μμ κ²½μ° μλ¬μ²λ¦¬λ₯Ό κ³ μΉλ €λ κ³Όμ μμ μ½λμ μμ λ νλ€... νν³..

&nbsp; userλ₯Ό κ΅³μ΄ μ°Ύμμ¬ νμκ° μμλ€...γγ

&nbsp; μλ¬μ κ²½μ° λΌμ°ν° μμμ μλ ₯κ°μ μ ν¨μ±κ³Ό λ‘κ·ΈμΈ μ λ¬΄λ₯Ό μ²΄ν¬ν΄μ€¬κΈ° λλ¬Έμ μ€λ₯κ° λ  μΌμ μλ²μμ DBμ μμ±μ λͺ» ν κ²½μ°λ°μ μλ€κ³  μκ°νλ€.

&nbsp; λ°λΌμ Error Handlerμμ default κ°μΈ 500μΌλ‘ μ²λ¦¬ν  μ μκ² νλ€.

<br>

# π &nbsp; μ λ¦¬

<br>

π &nbsp; HTTP Status Codeμ λν΄ μμλ³΄κ³  μ΄λ₯Ό μ΄μ©νμ¬ μ€μ  νλ‘μ νΈμ μ½λλ₯Ό λ¦¬ν©ν λ§ ν΄λ΄€λ€.

&nbsp; μ΄λ₯Ό ν΅ν΄ μ½λμ νλ¦μ νμνκΈ° μ¬μμ‘κ³ , λ§μ½ μλ¬κ° λ°μνλ€κ³  ν΄λ λΉ λ₯΄κ² μλ¬λ₯Ό νμν  μ μκ² λ€λ μκ°μ΄ λ€μλ€.
