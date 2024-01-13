# 📚 스택(Stacks)

- 데이터들의 모임
- 압축적인 데이터 구조
- 스택은 LIFO(Last In Fisrts Out, 후입선출) 원칙을 따름(가장 먼저 추가한 것이 가장 나중에 제거)
  > ✓ **스택의 세가지 제약**
  >
  > 1. 데이터는 스택의 끝에만 삽입할 수 있다.
  > 2. 데이터는 스택의 끝에서만 삭제할 수 있다.
  > 3. 스택의 마지막 원소만 읽을 수 있다.

## 📌 어디서 사용?

- 함수를 호출할 때
- Undo / Redo
- 인터넷 방문 기록

## 📌 배열로 스택 만들기

- push와 pop 메소드를 만든다.
- 이 때 O(1) 시간복잡도를 가지는 shift와 unshift 메소드 로직을 활용한다.
- 단일 연결 리스트 활용

```javascript
// Node 클래스는 스택의 각 요소를 나타냄
// value는 노드의 갑을 저장
// next는 다음 노드를 가리킴
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}
// 스택 클래스
class Stack {
  constructor() {
    this.first = null;
    this.last = null;
    this.size = 0;
  }
  // unshift 메서드와 유사(맨 앞에 추가)
  push(val) {
    // val 값을 가진 newNode 생성
    var newNode = new Node(val);
    // this.first 는 null 값인데 null은 falsy이다.
    // 따라서 ! 를 붙여 truthy 값으로 바꿔줘야 한다.
    // 스택이 비어있다면 새노드는 first=last
    if (!this.first) {
      this.first = newNode;
      this.last = newNode;
      // 스택에 노드가 있다면
      // 새 노드는 first 노드 앞에 추가
      // first 노드를 새 노드로 업데이트
    } else {
      var temp = this.first;
      this.first = newNode;
      this.first.next = temp;
    }
    return ++this.size;
  }
  // shift 메서드와 유사
  pop() {
    // 스택이 비어 있다면 null값 반환
    if (!this.first) return null;
    // 제거될 노드 저장
    var temp = this.first;
    // 스택에 노드가 하나만 있다면 first와 last를 null로 설정
    if (this.first === this.last) {
      this.last = null;
    }
    // 그렇지 않으면 first를 다음 노드로 업데이트
    this.first = this.first.next;
    this.size--;
    return temp.value;
  }
}
```

<img src="https://blog.kakaocdn.net/dn/m3XOD/btrAgozacDI/fRqJAKfWAaK9iwfEKIkdyk/img.png">

## 📌 스택의 Big O

- **삽입 : O(1)**
- **제거 : O(1)**
- 검색 : O(n)
- 접근 : O(n)

# 📚 큐(Queues)

- 데이터 구조로서 데이터를 추가,제거
- FIFO(First In First Out, 선입선출)의 데이터 구조
- ex) 온라인 게임 접속할 때 먼저 접속한 사람이 먼저 접속

```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class Queue {
  constructor() {
    this.first = null;
    this.last = null;
    this.size = 0;
  }
  // 큐의 끝에 새 노드 추가
  enqueue(val) {
    var newNode = new Node(val);
    // 큐가 비어 있다면 새 노드는 first, last
    if (!this.first) {
      this.first = newNode;
      this.last = newNode;
      // 큐에 이미 노드가 있다면
      // 새 노드는 last 다음에 추가
      // 따라서 새 노드를 마지막 노드로 업데이트
    } else {
      this.last.next = newNode;
      this.last = newNode;
    }
    return ++this.size;
  }

  // 큐의 맨 앞에서 노드 제거
  dequeue() {
    if (!this.first) return null;
    // 제거 될 노드 저장
    var temp = this.first;
    // 큐에 노드가 하나만 있다면 last는 null
    if (this.first === this.last) {
      this.last = null;
    }
    // 큐애 노드가 여러개 있다면
    // first를 다음 노드로 업데이트
    this.first = this.first.next;
    this.size--;
    return temp.value;
  }
}
```

<img src="https://blog.kakaocdn.net/dn/5NOv1/btqSTINnoq8/4f8bjzzf6W4POewlq8At31/img.png">

## 📌 큐의 Big O

- 삽입 : O(1)
- 제거 : O(1)
- 탐색 : O(n)
- 접근 : O(n) </br>
  => 탐색과 접근은 큐에서 사용하지 않음
