<br>

## 🔎  스키마 설계 팁!

<br>

👉  지금까지 populate를 이용하는 관계 방식과 문서를 내장하는 내장 형식을 살펴봤다.

Blog 예제에서는 내장 방식이 성능상 유리했지만 개발에 있어서 은탄환은 없듯이 꼭 내장만 좋은 건 아니다!

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cf825653-cc24-4d32-9cb8-748a67b4eae8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221203%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221203T063143Z&X-Amz-Expires=86400&X-Amz-Signature=4093657e57a583b9c74a6562670c0f2b2d2b61be6beb11a24dbcbe7e7c66c2e4&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

일단 1대 1 관계든, 1대 N 관계든간에 위의 조건을 생각하면서 짜면 좋다.

<br>

- 개별적으로 읽을 때도 있다??
  - Elice 1차 프로젝트때 했던 카테고리 스키마가 이에 해당하는 것 같다.

<br>

- 내장하려는 문서가 자주 바뀐다??
  - 내장은 CUD시에 더 작업을 많이 해야하기 때문에 그런 것 같다.

<br>

- 같이 불러올 때가 많다??
  - 데이터베이스에 많이 접근하는 건 좋지 않기 때문에 같이 불러올게 많다면 그냥 내장을 하는게 성능에 더 좋을 것 같다는 생각이 든다.

<br>

- 읽기 비중이 CUD보다 더 높다??
  - 위에서도 말한 내용이다.

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/faaa0ea0-8109-4ec9-9519-5f8dd236b370/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221203%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221203T063159Z&X-Amz-Expires=86400&X-Amz-Signature=e87a0994c7e9a9633f329c72bcc131b18fdb7b312d061adf0581dd311b069993&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

👉  1대 다의 관계에서 설계를 할 때의 팁이다!

<br>

- N이 100보다 작으면 내장이 더 좋다.
- 100 < N < 1000이면 id만 내장하는게 좋다.
- 1000 < N이면 관계로 짜는게 더 좋다. 또한 N을 다양한 조건으로 검색을 해야하는 경우도 관계로 짜는 것이 더 좋다.
