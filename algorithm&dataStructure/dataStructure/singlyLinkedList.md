<br>

# 📌  단방향 연결리스트

<br>

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b59e69d6-94c5-435f-9bda-4b11e16ae47b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230119T091613Z&X-Amz-Expires=86400&X-Amz-Signature=b457d8dbf0f53a7fbbb6cdb02b876adba71005d2673030154e6a8f3221f1e4d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>

👉  단방향 연결리스트는 데이터와 다음 노드를 가리키는 노드들이 단뱡향으로 연결되어있는 자료구조이다.

Array와의 차이점은 다음과 같다.

<br>

- Array는 인덱스가 존재하기 때문에 원하는 자료에 직접 접근할 수 있다. 그러나 단방향 연결리스트는 그럴 수 없다.

  - 건물의 엘리베이터를 예시로 들 수 있다.

<br>

## 🔎  Singly Linked Lists 구현하기

<br>

### ✏️  Singly Linked List Class

<br>

```tsx

// 노드 인스턴스 생성을 위한 class를 정의했다.
class Node{
	constructor(value{
		this.value = value;
		this.next = null;
	}
}

// 단일 연결리스트 생성을 위한 class를 정의했다.
class SinglyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }
}
```

<br>

### ✏️  Push()

<br>

```tsx
push(value) {
    const newNode = new Node(value);

    if (this.head === null) {
      this.head = newNode;
      this.tail = newNode;
      this.length++;
    } else {
      this.tail.next = newNode;
      this.tail = newNode;
      this.length++;
    }

    return true;
  }
```

<br>

- head가 null일 경우

  - head가 newNode를 가리키게 한다.
  - tail이 newNode를 가리키게 한다.
  - length에 1을 더해준다.

<br>

- head가 null이 아닐 때

  - tail.next가 newNode를 가리키게 한다.
  - tail이 newNode를 가리키게 한다.
  - length에 1을 더해준다.

<br>

### ✏️  Pop()

<br>

```tsx
pop() {
    if (!this.head) return 'list empty';

    let current = this.head;
    let newTail = current;

    while (current.next) {
      newTail = current;
      current = current.next;
    }

    this.tail = newTail;
    this.tail.next = null;
    this.length--;

    if (this.length === 0) {
      this.head = null;
      this.tail = null;
    }
    return current;
  }
```

<br>

👉  pop()은 3가지의 경우를 생각해봤다.

<br>

- head가 null일 경우
  - head가 null이면 리스트가 비었다는 뜻이므로 pop 수행 불가

<br>

- List에 head 1개만 존재할 경우
  - head를 return
  - 이때, List의 length가 0이 되어야 한다.
    즉, head와 tail을 0으로 만들어줘야 한다.

<br>

- List에 여러 개의 Node가 존재할 때
  - Node를 타고 가면서 마지막에서 두 번째 노드를 찾는다.(beforeTarget)
  - tail이 beforeTarget을 가리키게 한다.
  - tail이 가리키는 beforeTarget 노드의 next를 null로 만들어준다.
  - length에 1을 빼준다.

<br>

### ✏️  Shift()

<br>

```tsx
shift() {
    if (this.head === null) return 'List empty';

    const result = this.head;
    this.head = this.head.next;
    this.length--;

    if (this.length === 0) {
      this.head = null;
      this.tail = null;
    }

    return result;
  }
```

<br>

👉  Shift()는 맨 앞의 노드 즉, 헤드를 List에서 제외하고 반환해주는 메서드이다.

<br>

- head가 head.next를 가리키게 한다.
- length에 1을 빼준다.

<br>

### ✏️  Unshift()

<br>

```tsx
unshift(value) {
    const newNode = new Node(value);

    if (!this.head) {
      this.head = newNode;
      this.tail = newNode;
      this.length++;
    } else {
      newNode.next = this.head;
      this.head = newNode;
      this.length++;
    }
  }
```

<br>

👉  unshift()는 List의 맨 앞에 node를 추가해주는 메서드이다.

<br>

- Node를 새로 생성해준다.(newNode)
- newNode.next가 현재 List의 맨 앞인 head를 가리키게 한다.
- head가 NewNode를 가리키게 한다.
- length에 1을 더해준다.

<br>

### ✏️  Get()

<br>

```tsx
get(index) {
    if (index < 0 || index >= this.length) return null;
    if (!this.head) return 'List empty';

    let current = this.head;

    for (let i = 0; i < index; i++) {
      current = current.next;
    }

    return current;
  }
```

<br>

👉  get()은 파라미터로 넘어온 index의 수만큼 next를 타고 가서 그 위치에 있는 node를 반환해주는 메서드이다.

<br>

- index가 유효하지 않으면 null을 반환해준다.
- 반복문을 돌리면서 next를 타고 간다.
- index 위치에 도달하면 그 위치의 node를 반환한다.

<br>

### ✏️  Set()

<br>

```tsx
set(index, value) {
    let foundNode = this.get(index);

    if (foundNode) {
      foundNode.value = value;
      return true;
    }

    return false;
  }
```

<br>

👉  set()은 index의 위치에 있는 node의 value를 파라미터로 넘어온 value로 바꿔주는 메서드이다.

<br>

- get()을 활용해 index 위치에 있는 node를 찾아준다.
- 찾아온 노드가 없다면 false를 return한다.
- 찾아온 노드가 존재하면 그 노드의 value 값을 value로 바꿔준다.

<br>

### ✏️  Insert()

<br>

```tsx
insert(index, value) {
    if (index < 0 || index > this.length) return false;
    if (index === 0) return this.unshift(value) ? true : false;
    if (index === this.length) return this.push(value) ? true : false;

    let newNode = new Node(value);
    let prevNode = this.get(index - 1);

    newNode.next = prevNode.next;
    prevNode.next = newNode;
    this.length++;

    return true;
  }
```

<br>

👉  insert()는 index 위치에 값이 value인 node를 추가해주는 메서드이다.

- index가 유효하지 않은 경우

  - false를 반환한다.

  <br>

- index가 0인 경우

  - 맨 앞에 추가하는 것이기 때문에 unshift()를 호출한다.

  <br>

- index가 length와 같은 경우

  - 맨 뒤에 추가하는 것이기 때문에 push()를 호출한다.

  <br>

- 그 외 index가 유효한 경우

  - 추가할 위치의 바로 전 node를 get()을 이용해 찾는다.
  - newNode.next가 prevNode.next를 가리키도록 한다.
  - prevNode.next가 newNode를 가리키게 한다.
  - length에 1을 더해준다.

  <br>

추가로 모든 함수의 결과값의 형은 일관되게 하는 것이 좋다는 것을 알게 되었다.

<br>

### ✏️  Remove()

<br>

```tsx
remove(index) {
    if (index < 0 || index >= this.length) return null;
    if (index === 0) return this.shift();
    if (index === this.length - 1) return this.pop();

    let prev = this.get(index - 1);
    let target = prev.next;

    prev.next = target.next;
    this.length--;

    return target;
  }
```

<br>

👉  remove()는 index 위치의 node를 제거해주는 메서드이다.

<br>

- index가 유효하지 않은 경우

  - false를 반환한다.

  <br>

- index가 0인 경우

  - 맨 앞의 노드를 제거하는 것이므로 shift()를 호출한다.

  <br>

- index가 length-1과 같은 경우

  - 맨 뒤를 제거하는 것이기 때문에 pop()를 호출한다.

  <br>

- 그 외 index가 유효한 경우
  - 추가할 위치의 바로 전 node를 get()을 이용해 찾는다.
  - prevNode.next가 target.next를 가리키게 한다.
  - length에 1을 빼준다.
