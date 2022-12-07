<br>

# 📌  Sharded cluster

<br>

## 🔎  Vertical Scale

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/63d12cc2-134f-45db-9eec-184d447ce2a0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221207%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221207T134831Z&X-Amz-Expires=86400&X-Amz-Signature=e6bd9eb25b8de9f8f4721e7e31055feeb14da385049a7f0e30c319204eefe27d&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

👉  수직적 확장은 현재 DB의 사양을 높이는 것이다.
DB 사양을 높인다 해도 모든 데이터는 Primary DB 안에 존재한다.

<br>

## 🔎  Vertical Scale

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7dfe65c7-f59f-4b87-9abc-ca2f55702eb4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221207%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221207T134845Z&X-Amz-Expires=86400&X-Amz-Signature=dc9f0e91931c4b354b048f4bf2eb210a2e6557e49525ea6453e668a1ec6ba3d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

👉  수평적 확장은 그냥 DB를 따로 두는 것이다. 가장 큰 장점은 한계가 없다는 것이다.

Replica Set을 여러 개 두는 걸 Sharded Cluster라고 한다.

<br>

# 📌  정리

<br>

- RDB가 수평적 확장이 어려운 이유에 대해 알 수 있게 되었다.
  - RDB는 정규화와 관계를 통해 데이터를 가져오는데, 만약 DB가 여러개라면 Consistency에 영향을 받을 수 있고, 마냥 성능이 좋아진다는 보장을 못 할 수 있다.
  - RDB는 Auto Increment 즉 int인 id를 사용하는데, 만약 DB가 여러개라면 id가 꼬일 수 있다. 이에 반해 MongoDB는 ObjectId라는 랜덤값을 사용하기 때문에 더 유리하다.
