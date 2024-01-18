# 연결 리스트란?

<img src="../images/연결 리스트.jpg">

- 문자열, 숫자 등 무엇이던 원하는 데이터를 저장하여 리스트를 표현하는 자료구조
- 순서에 따라 다수의 데이터를 저장
- 인덱스 X
- 다수의 노드들로 구성되고, 각각의 노드는 문자열 혹은 숫자와 같은 하나의 데이터 요소를 저장
  > 💡 **노드**란? 컴퓨터 메모리 곳곳에 흩어져 있는 데이터 조각
- 마치 기차가 연결된 것 같은 구조
- 노드는 각각 흩어져 있는데 컴퓨터는 어떤 노드들이 같은 연결 리스트에 속하는지 어떻게 알까? => 노드는 연결 리스트 내에 다음 노드의 메모리 주소도 포함한다.
- 다음 노드의 메모리 주소로의 포인터(추가 데이터)를 **링크(link)** 라고 부른다.

> - Head : 시작 노드
> - Tail : 마지막 노드
> - Length : 리스트의 길이

# 단순 연결 리스트(Singly Linked Lists)

<img src="https://velog.velcdn.com/images/sangbin2/post/8e39992e-9898-46ee-bc17-732363404983/image.png">
- 각 노드가 다음 노드로 오직 단방향으로만 연결

```javascript
// 데이터 조각 - val
// 다음 노드의 참조값 - next
class Node {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}
// 출력 - Node {val: undefined, next: null}

class SinglyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }
  // 1. Push 메소드
  //리스트 맨끝에 노드 추가
  push(val) {
    // val 값이 저장된 새 Node 객체 생성
    var newNode = new Node(val);
    // 리스트가 비어 있는 경우
    if (!this.head) {
      // 처음이자 마지막 노드이기 때문에 head와 tail은 같다.
      this.head = newNode;
      this.tail = this.head;
    } else {
      // 새 노드 연결
      this.tail.next = newNode;
      this.tail = newNode;
    }
    // 새 노드 추가
    this.length++;
    return this;
  }
  // 2. Pop 메소드
  // 리스트 맨 끝에 노드 삭제
  pop() {
    // 노드가 없는 경우(제거할 것이 없는 경우) undefined 반환
    if (!this.head) return undefined;
    var current = this.head;
    var newTail = current;
    // 마지막 노드 찾기
    while (current.next) {
      newTail = current;
      current = current.next;
    }
    // 마지막 노드 제거
    // this.tail을 newTail로 업데이트하여 새로운 마지막 노드로 설정
    this.tail = newTail;
    this.tail.next = null;
    this.length--;
    // 리스트가 비었는지 확인
    if (this.length === 0) {
      this.head = null;
      this.tail = null;
    }
    // 제거된 마지막 노드 반환
    return current;
  }
  // 3. shift 메소드
  // 리스트 시작 부분(첫번째 노드) 제거
  shift() {
    if (!this.head) return undefined;
    var currentHead = this.head;
    // this.head를 다음 노드로 업데이트
    this.head = currentHead.next;
    this.length--;
    // 리스트가 비어있는지 확인
    if (this.length === 0) {
      this.tail = null;
    }
    return currentHead;
  }
  // 4. unshift
  // 새로운 노드를 리스트 맨 앞에 추가
  unshift(val) {
    var newNode = new Node(val);
    // 리스트가 비어 있는 경우
    if (!this.head) {
      this.head = newNode;
      this.tail = this.head;
      // 리스트에 노드가 있을 경우
    } else {
      // 새 노드가 기존의 첫번째 노드 가리킴
      newNode.next = this.head;
      this.head = newNode;
    }
    this.length++;
    return this;
  }
  // 5. get 메소드
  // 리스트의 지정된 위치에 있는 노드 반환
  get(index) {
    // 노드 존재하지 않을 때
    if (index < 0 || index >= this.length) return null;
    var counter = 0;
    var current = this.head;
    // counter가 index와 같아질 때까지 작동
    while (counter !== index) {
      current = current.next;
      counter++;
    }
    return current;
  }
  // 6. set 메소드
  // 리스트의 특정 위치에 있는 노드값을 업데이트
  set(index, val) {
    // 노드의 인덱스 찾기
    var foundNode = this.get(index);

    if (foundNode) {
      // 새로운 값으로 업데이트
      foundNode.val = val;
      return true;
    }
    return false;
  }
  // 7. insert 메소드
  // 리스트의 특정 위치에 새로운 노드 삽입
  insert(index, val) {
    if (index < 0 || index > this.length) return false;
    // 리스트 끝에 삽입
    if (index === this.length) return !!this.push(val);
    // 리스트 처음에 삽입
    if (index === 0) return !!this.unshift(val);

    // 리스트 중간에 삽입
    var newNode = new Node(val);
    //삽입할 위치 바로 이전의 노드(prev)를 찾음
    var prev = this.get(index - 1);
    // 삽입할 위치의 현재 노드
    var temp = prev.next;
    // 이전 노드가 새 노드를 가리키게 함
    prev.next = newNode;
    // 새 노드가 원래 삽입 위치의 노드를 가리키게 함
    newNode.next = temp;
    this.length++;
    return true;
  }
  // 8. Remove 메소드
  // 리스트에서 특정 인덱스 위치의 노드 제거
  remove(index) {
    if (index < 0 || index >= this.length) return undefined;
    // 시작 부분에서 제거
    if (index === 0) return this.shift();
    // 마지막 부분에서 제거
    if (index === this.length - 1) return this.pop();
    // 중간 부분에서 제거
    // 제거할 노드의 바로 이전 노드를 찾음
    var previousNode = this.get(index - 1);
    // 재거할 노드를 저장
    var removed = previousNode.next;
    // 제거될 노드를 건너뛰고 그 다음 노드를 가리키도록 함
    previousNode.next = removed.next;
    this.length--;
    return removed;
  }
  // 9. Reverse 메소드
  // 리스트의 순서를 뒤집음
  // head와 tail을 서로 바꾸고, 각 노드의 next 포인터를 변경하여 순서를 반대로 만든다.
  reverse() {
    // head와 tail 교환
    var node = this.head;
    this.head = this.tail;
    this.tail = node;
    var next;
    var prev = null;
    for (var i = 0; i < this.length; i++) {
      next = node.next;
      // 노드의 연결 방향을 반대로 뒤집음
      node.next = prev;
      // prev를 현재노드로 업데이트
      prev = node;
      // node를 next로 업데이트 하여 다음노드로 이동
      node = next;
    }
    return this;
  }
}
var list = new SinglyLinkedList();
```

## 단일 연결리스트의 Big O

- 삽입 : O(1)
  - 제일 앞이나 뒤에 노드를 추가 할 때
  - 배열은 O(n)이기 때문에 삽입을 할 경우 단일 연결리스트가 유리
- 제거 : O(1) or O(n)
  - 첫번째 노드 제거 0(1), 마지막 노드 제거 O(n)
- 검색 : O(n)
- 접근 : O(n)
  - 배열은 인덱스로 접근을 하기 때문에 O(1) 소요

=> 삽입, 제거에서는 배열보다 효율적
