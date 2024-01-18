# 📚 이진 힙(Binary Heaps)

- 힙의 한 종류이며, 힙은 트리의 한 종류이다.
- 이진 탐색 트리와 매우 비슷하다.
- 각 노드는 2개 이하의 자식을 가진다.
- 형제 노드 사이에는 순서가 정해져 있지 않다.
- 힙은 항상 왼쪽에서 오른쪽으로 채워지기 때문에 한쪽에 치우친 트리는 나오지 않는다.

![](https://www.programiz.com/sites/tutorial2program/files/max-heap-min-heap.png)

## 📌 최대 이진 힙(Max Binary Heap)

![](https://www.programiz.com/sites/tutorial2program/files/heapfy-root-element-when-subtrees-are-max-heaps.png)

- 가장 큰 노드가 최상위에 있다.
- 부모 노드가 자식 노드보다 항상 더 크다.
- 가능한 가장 적은 최적의 공간을 차지한다.

## 📌 최소 이진 힙(Min Binary Heap)

- 가장 작은 노드가 최상위에 있다.
- 부모 노드가 자식 노드보다 항상 더 작다.

## 📌 부모와 자식 노드가 서로의 위치를 찾는 규칙

### 최대 이진 힙

<img src="images/이진 힙 규칙.png">

- 자식의 위치를 찾고 싶을 때: 왼쪽 자식은 부모의 인덱스(n)에 2n+1, 오른쪽 자식은 2n+2

<img src="images/이진 힙 규칙2.png">
- 부모의 위치를 찾고 싶을 때: 자식의 인덱스(n)에 (n-1)/2

## 📌 최대 이진 힙 - Insert 메서드

- 삽입할 노드를 인덱스 맨 마지막에 추가한다.
- 부모 노드와 비교하여 올바른 위치에 도달할 때까지 노드를 교환한다.
  <img src="images/최대이진힙1.png">
  <img src="images/최대이진힙2.png">
  <img src="images/최대이진힙3.png">

```javascript
class MaxBinaryHeap {
  constructor() {
    this.values = [];
  }
  // 1. 삽입할 요소를 배열에 추가하고 bubbleUp 함수 호출
  insert(element) {
    this.values.push(element);
    this.bubbleUp();
  }
  // 2. 버블업(교체)메서드
  bubbleUp() {
    // 삽입한 노드의 인덱스를 찾음
    let idx = this.values.length - 1;
    // 삽입한 노드를 저장
    const element = this.values[idx];
    // 부모 노드와 비교하며 위치 바꿈
    while (idx > 0) {
      // 부모 노드의 인덱스 계산
      let parentIdx = Math.floor((idx - 1) / 2);
      // 부모 노드 저장
      let parent = this.values[parentIdx];
      // 새 노드가 부모 노드보다 작거나 같으면 순회 중지(올바른 위치)
      if (element <= parent) break;
      // 새 노드가 부모 노드보다 크다면 두 노드의 위치 바꿈
      this.values[parentIdx] = element;
      this.values[idx] = parent;
      idx = parentIdx;
    }
  }
}
```

## 📌 최대 이진 힙 - ExtractMax 메서드

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99A3B23359848EC70F)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F997C2C3359848FEF20)

- 삭제 연산은 최댓값(최상위 루트)을 삭제하는 것이다.
- 최댓값이 삭제되면 최하단에 있는 노드를 루트 노드에 가져와 자식 노드들과 교환한다.

```javascript
extractMax() {
  // 이진 힙의 max와 end 변수 정의
    const max = this.values[0];
    const end = this.values.pop();
  // pop한 후에 남은 요소가 있으면
    if (this.values.length > 0) {
      // root 자리에 가장 최근에 삽입됐던 end를 넣고
      this.values[0] = end;
      // root 자리에 올라간 end의 sinkDown()을 시작한다.
      this.sinkDown();
    }
  // pop한 후에 남은 요소가 없으면 바로 return하고 종료한다.
    return max;
}

sinkDown() {
 // 1. 인덱스를 0으로 초기화
  let idx = 0;
  // 힙의 전체 길이
  const length = this.values.length;
  // 2. 트리 순회
  while (true) {
    // 현재 노드의 값
    const element = this.values[idx];
    // 3. 자식 노드의 인덱스 계산
    const leftChildIdx = 2 * idx + 1;
    const rightChildIdx = 2 * idx + 2;
    let leftChild;
    let rightChild;
    let swap = null;

// 4. 자식 노드와 비교하여 위치 교환
// 왼쪽 자식 노드가 존재하는지 확인
if (leftChildIdx < length) {
      leftChild = this.values[leftChildIdx];
      // 왼쪽 자식 노드값이 현재 노드보다 크면
      // 왼쪽 자식의 인덱스 저장
      if (leftChild > element) {
        swap = leftChildIdx;
      }
    }
    // 오른쪽 자식이 존재하는지 확인
    if (rightChildIdx < length) {
      rightChild = this.values[rightChildIdx];
      // 오른쪽 자식 노드의 값이 현재 요소보다 큰 경우
      // 그리고 이전에 왼쪽 자식 노드와의 위치 교환을 하지 않았거나(swap === null)
      // 오른쪽 자식 노드가 왼쪽 자식 노드보다 큰 경우
      // swap 변수에 오른쪽 자식 노드의 인덱스를 할당
      if ((swap === null && rightChild > element) || (swap !== null && rightChild > leftChild)) {
        swap = rightChildIdx;
      }
    }
// 위치를 바꿀 필요가 없을 때 루프 종료
if (swap === null) break;
    // 그렇지 않다면 현재 노드와 바꿀 노드의 위치를 바꾼다.
    [this.values[idx], this.values[swap]] = [this.values[swap], this.values[idx]];
    // idx 를 swap 으로 업데이트
    // 자식 노드로 이동함
    idx = swap;
  }
}
```

## 📌 우선순위 큐(Priority Queue)

- 각 요소가 그에 해당하는 우선순위를 가지는 데이터 구조
- 더 높은 우선순위를 가진 요소가 더 낮은 우선순위를 가진 요소보다 먼저 처리된다.
- ex) 병원에서 응급환자
- 높은 우선순위를 가진 요소는 낮은 우선순위를 가진 요소보다 앞에 온다.
- 우선순위 큐는 보통 힙을 사용하지만 둘은 상관관계가 없다.
- 하지만 힙이 더 좋은 성능(log n)을 가지기 때문에 힙으로 우선순위 큐를 구현해본다.

- 아래 코드는 이진힙 코드와 거의 같다.

```javascript
class PriorityQueue {
  constructor() {
    this.values = [];
  }
  enqueue(val, priority) {
    // 새 노드 추가
    let newNode = new Node(val, priority);
    this.values.push(newNode);
    this.bubbleUp();
  }
  bubbleUp() {
    let idx = this.values.length - 1;
    const element = this.values[idx];
    while (idx > 0) {
      let parentIdx = Math.floor((idx - 1) / 2);
      let parent = this.values[parentIdx];
      // 현재 노드의 우선순위가 부모 노드의 우선순위보다 크거나 같다면
      if (element.priority >= parent.priority) break;
      this.values[parentIdx] = element;
      this.values[idx] = parent;
      idx = parentIdx;
    }
  }
  dequeue() {
    const min = this.values[0];
    const end = this.values.pop();
    if (this.values.length > 0) {
      this.values[0] = end;
      this.sinkDown();
    }
    return min;
  }
  sinkDown() {
    let idx = 0;
    const length = this.values.length;
    const element = this.values[0];
    while (true) {
      let leftChildIdx = 2 * idx + 1;
      let rightChildIdx = 2 * idx + 2;
      let leftChild, rightChild;
      let swap = null;

      if (leftChildIdx < length) {
        leftChild = this.values[leftChildIdx];
        // 왼쪽 자식의 우선순위가 현재 요소의 우선순위보다 크면
        if (leftChild.priority > element.priority) {
          swap = leftChildIdx;
        }
      }
      if (rightChildIdx < length) {
        rightChild = this.values[rightChildIdx];
        if (
          (swap === null && rightChild.priority < element.priority) ||
          (swap !== null && rightChild.priority < leftChild.priority)
        ) {
          swap = rightChildIdx;
        }
      }
      if (swap === null) break;
      this.values[idx] = this.values[swap];
      this.values[swap] = element;
      idx = swap;
    }
  }
}

class Node {
  constructor(val, priority) {
    this.val = val;
    this.priority = priority;
  }
}
```

## 📌 이진 힙의 Big O

- 삽입 : O(lon N)
- 제거 : O(lon N)
- 탐색 : O(N)
