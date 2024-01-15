# 🌳 트리(Tree)

![트리](https://blog.kakaocdn.net/dn/eeoNuG/btq1Eo7t7Xk/0bPk7BzhiruKSsgtiubvK0/img.png)

- parent/child 관계 안에서 노드로 이루어진 데이터 구조
- 연결리스트처럼 노드를 가지고 있으며, 노드는 여러개의 노드를 가리킬 수 있다.
- 비선형 구조(경로가 여러개)
- 노드는 부모-자식 관계에 따라 자식인 노드만 가리킬 수 있다.
- 출발 지점(루트)은 하나여야 한다.

## 🌱 트리 용어

- Root : 부모가 없는 최상위 노드
- Child : 루트에서 멀어지는 방향으로 연결된 노드
- Parent : 자식의 반대 개념
- Sibilngs : 같은 부모를 가진 노드
- Leaf : 자식이 없는 노드
- Edge : 노드와 노드 사이를 연결하는 화살표

## 🌱 트리는 어디서 사용되는가?

- HTML DOM
- 네트워크 라우터
- 추상 구문 트리
- AI
- 운영체제에서 폴더가 설계된 방식
- JSON

> ✅ **트리의 종류**
>
> - 이진 트리 : 각 노드가 최대 두 개의 자식을 가져야 한다.
> - 이진 탐색 트리

# 🌳 이진 탐색 트리(Binary Search Tree, BST)

![이진탐색트리](https://blog.kakaocdn.net/dn/b50CLv/btrfWceBceL/lLCTey5Fyvc5X92i5MaLk1/img.png)

- 이진 트리의 종류(이진 트리 중에서 탐색에 제일 용이)
- 모든 노드는 최대 두개의 자식을 가짐
- 데이터를 비교해서 정렬 가능하게 저장
- 부모 노드의 왼쪽에 있는 모든 노드는 부모보다 작고, 오른쪽에 있는 노드는 부모보다 큼

## 🌱 기본구조

```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
}
```

## 🌱 Insert 메서드

```javascript
 insert(value){
    // 1. 새 노드 생성
        var newNode = new Node(value);
        // 2. 루트노드가 비어 있다면
        // 루트 노드를 새노드로 설정
        if(this.root === null){
            this.root = newNode;
            return this;
        }
        // 3. 값 비교 및 적절한 위치 탐색
        // current를 현재 루트 노드로 설정
        var current = this.root;
        // 루프를 통해 새 노드의 위치 찾기
        while(true){
            // 중복 값 방지
            // 새 값이 현재 노드와 같으면 undefined 반환
            if(value === current.value) return undefined;
            // 4. 왼쪽 자식 노드로 삽입
            // 새 값이 현재 노드의 값보다 작고
            if(value < current.value){
                // 왼쪽 자식 노드가 비어있다면
                // 새 노드 삽입
                if(current.left === null){
                    current.left = newNode;
                    return this;
                }
                // 왼쪽 자식 노드가 이미 있다면
                // current를 현재 노드의 왼쪽 자식으로 업데이트
                current = current.left;
                // 5. 오른쪽 자식 노드로 삽입
                // 위와 동일
            } else {
                if(current.right === null){
                    current.right = newNode;
                    return this;
                }
                current = current.right;
            }
        }
    }
```

## 🌱 Find 메서드

```javascript
find(value){
        // 1. 루트노드가 비어있다면 false 반환
        if(this.root === null) return false;
        // 2. 변수 초기화
        // current를 루트 노드로 설정
        // found는 원하는 값을 찾았는지를 나타냄. 초기값은 false
        var current = this.root,
            found = false;
        // 3. 값 순회
        // 루트 노드가 존재하고 찾는 값이 없을 경우를 반복
        while(current && !found){
            // 찾는 값이 현재 노드의 값보다 작으면
            // 현재 노드를 현재 노드의 왼쪽 자식 노드로 업데이트
            if(value < current.value){
                current = current.left;
            // 찾는 값이 현재 노드의 값보다 크면
            // 현재 노드를 현재 노드의 오른쪽 자식 노드로 업데이트
            } else if(value > current.value){
                current = current.right;
            // 찾는 값과 현재 노드의 값이 같다면 found = true
            } else {
                found = true;
            }
        }
        // 4. 결과 반환
        // 찾는 값이 없다면 undefined 반환
        // !found는 found의 불린 값이 true일 때 false를, false일 때 true를 반환. 즉 false인게 맞다면! 반환
        if(!found) return undefined;
        // 찾은 노드 반환
        return current;
    }
```

## 🌱 이진 탐색 트리의 Big O

- 삽입 : O(log n)
- 탐색 : O(log n)
