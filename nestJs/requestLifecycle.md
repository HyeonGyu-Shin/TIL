<br>

## 🔎  Nest의 Request Lifecycle

<br>

👉  Nest에 요청이 들어왔을 때, 응답을 할 때까지 생명주기를 가진다.
이에 대해 알아보자!

<br>

1. Incoming request → 요청이 들어온다.
2. middleware → controller에 도달하기 전 app에 붙여둔 middleware가 실행이 된다.
3. guards → 추후 배울 예정
4. intercept → middleware를 거친 후, handler 함수가 실행되기 전에 interceptor가 실행이 된다.
5. pipes → handler 함수의 body 부분이 실행되기 전에 pipes가 실행이 된다.
6. controller
7. service
8. intercept → 들어온 요청이 원하는 처리를 끝마치면 이 곳에 있는 콜백이 실행된다.
9. 에러 처리 필터로 1~8까지 오류가 생기면 바로 이 곳으로 제어권이 넘어온다.
10. server response -성공적으로 요청을 끝마치고 그에 대한 결과값을 반환해준다.

<br>

이처럼 Nest는 많은 단계를 거치고, 세분화된 순간순간의 단계에서 로직을 더 풍부하게 할 수 있겠다는 생각이 들었다.
