# 이중 연결 리스트(Doubly Linked Lists)

![이중 연결 리스트](https://velog.velcdn.com/images%2Fjangws%2Fpost%2F6e536fa6-3f53-4dc8-af5c-5ae285c49372%2F3.jpg)

- 단일 연결리스트에서 포인터 하나를 더하는 것과 같다. 즉, 노드에 포인터가 두개가 존재한다.(양방향)
- 단일 연결 리스트와 비교했을 때 메모리가 더 많이 든다. More memory === More Flexibility(상쇄)

```javascript
class Node {
  constructor(val) {
    this.val = val;
    this.next = null;
    this.prev = null;
  }
}

class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }
}
```

## 📌 Push

<img src="../images/이중 연결 리스트-Push.jpg">

```javascript
// 1. Push 메소드
push(val) {
  // 추가될 노드 생성
  var newNode = new Node(val);
  // 리스트가 비어 있는 경우 모두 새 노드를 가리킴
  if (this.length === 0) {
    this.head = newNode;
    this.tail = newNode;
  } else {
    // 리스트에 노드가 있는 경우
    this.tail.next = newNode;
    newNode.prev = this.tail;
    this.tail = newNode;
  }
  this.length++;
  return this;
}

```

## 📌 Pop

<img src="../images/이중 연결 리스트-Pop.jpg">

```javascript
// 2. Pop 매소드
 pop(){
        if(!this.head) return undefined;
        // 제거할 마지막 노드 저장
        var poppedNode = this.tail;
        // 리스트에 노드가 하나만 있을 때 제거
        if(this.length === 1){
            this.head = null;
            this.tail = null;
            // 여러 노드가 있을 떄
        } else {
            this.tail = poppedNode.prev;
            this.tail.next = null;
            poppedNode.prev = null;
        }
        this.length--;
        return poppedNode;
    }
```

## 📌 Shift

<img src="../images/이중 연결 리스트-Shift.jpg">

```javascript
if (this.length === 0) return undefined;
// 제거될 현재 head 저장
var oldHead = this.head;
// 리스트에 노드가 하나만 있을 경우
if (this.length === 1) {
  this.head = null;
  this.tail = null;
  // 여러 노드가 있을 경우
} else {
  this.head = oldHead.next;
  this.head.prev = null;
  oldHead.next = null;
}
this.length--;
return oldHead;
```

## 📌 unshift

```javascript
 unshift(val){
        var newNode = new Node(val);
        // 리스트가 비어있는 경우
        if(this.length === 0) {
            this.head = newNode;
            this.tail = newNode;
            // 리스트에 노드가 있는 경우
        } else {
            // 현재 head의 prev(포인터)를 newNode로 설정
            this.head.prev = newNode;
            // newNode의 next(포인터)를 head로 설정
            newNode.next = this.head;
            // head를 newNode로 업데이트
            this.head = newNode;
        }
        this.length++;
        return this;
    }
```

<img src="../images/이중 연결 리스트-unshift.jpg">

## 📌 get

```javascript
get(index){
    // 인덱스가 0보다 작고, 리스트이 길이보다 크거나 같으면 null 반환
        if(index < 0 || index >= this.length) return null;
        var count, current;
        // 리스트 앞부분에서 탐색
        if(index <= this.length/2){
            // count를 0으로 초기화, current를 head로 설정
            count = 0;
            current = this.head;
            // 주어진 인덱스와 맞지 않는다면 다음 노드로 이동 후 카운트 증가
            while(count !== index){
                current = current.next;
                count++;
            }
            // 리스트의 뒷부분에서 탐색
        } else {
            count = this.length - 1;
            current = this.tail;
            // 주어진 인덱스와 맞지 않는다면 이전 노드로 이동 후 카운트 감소
            while(count !== index){
                current = current.prev;
                count--;
            }
        }
        return current;
    }
```

## 📌 set

```javascript
set(index, val){
    // get함수 호풀하여 인덱스 찾음
        var foundNode = this.get(index);
        // 노드를 찾았다면 val값을 새로운 값으로 업데이트
        if(foundNode != null){
            foundNode.val = val;
            return true;
        }
        return false;
    }
```

## 📌 insert

```javascript
    insert(index, val){
        // 인덱스가 0보다 작고 리스트의 길이보다 크면 false 반환
        if(index < 0 || index > this.length) return false;
        // <리스트의 시작 또는 끝에 삽입하는 경우>
        // 인덱스가 0이라면(노드가 하나 있다면) unshift 메소드를 사용하여 시작 부분에 삽입
        // !!을 통해 부울 값으로 변환
        if(index === 0) return !!this.unshift(val);
        // 인덱스와 리스트 길이가 같다면 push 메소드를 사용하여 마지막 부분에 삽입
        if(index === this.length) return !!this.push(val);

        // <리스트 중간에 삽입하는 경우>
        // 새 노드 생성
        var newNode = new Node(val);
        // 삽입 위치 바로 이전 노드를 찾음
        var beforeNode = this.get(index-1);
        // afterNode에 다음 노드 저장
        var afterNode = beforeNode.next;

        // 노드 연결
        // beforeNode의 next를 새 노드로 설정
        // 새 노드의 prev를 beforeNode로 설정
        beforeNode.next = newNode, newNode.prev = beforeNode;
        newNode.next = afterNode, afterNode.prev = newNode;
        this.length++;
        return true;
    }
```

<img src="../images/이중 연결 리스트-insert.jpg">

## 📌 remove

```javascript
remove(index) {
    // 인덱스가 0보다 작고 리스트이 길이보다 클 때 undefined 반환
    if (index < 0 || index > this.length) return undefined;
    // <리스트 시작 또는 끝에서 제거 하는 경우>
    // 인덱스가 0일 때, shift 메소드를 통헤 시작 부분에 노드 제거
    if (index === 0) return this.shift();
    // 인덱스가 리스트 길이의 마지막이면 리스트 끝에서 노드 제거
    if (index === this.length - 1) return this.pop();

    // 리스트 중간에서 제거
    // get 메소드를 통헤 제거할 노드 찾음
    var removedNode = this.get(index);
    // removeNode의 이전노드가 removeNode의 다음 노드를 가리키도록 함
    removedNode.prev.next = removedNode.next;
    removedNode.next.prev = removedNode.prev;

    // 제거된 노드와의 연결 해제
    removedNode.next = null;
    removedNode.prev = null;
    this.length--;
    return removedNode;
  }

```

<img src="../images/이중 연결 리스트-remove.png">

## 📌 이중 연결 리스트의 Big O

- 삽입 : O(1)
- 제거 : O(1)
- 탐색 : O(n)
- 접근 : O(n)

## 📌 리뷰

- 이중 연결 리스트는 전에 있는 노드를 가리키는 포인터가 하나 더 있다는 점만 빼면 단일 연결 리스트와 똑같다.
- 노드를 찾는데 절반의 시간이 걸리기 때문에 더 나은 성능을 발휘한다.
- 하지만 추가로 만든 포인터는 메모리를 더 소모한다.
