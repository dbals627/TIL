# 🌳 트리 순회(Tree traversal)

- 모든 트리에서 사용할 수 있는 개념

## 🌱 너비 우선 탐색(BFS, Breadth-first Search)

![](https://skilled.dev/images/tree-bfs.png)

- 같은 레벨에 있는 모든 노드를 거쳐가야 함
- 수평으로 작업

```javascript
class Node {
    constructor(value){
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTree {
    constructor(){
        this.root = null;
    }
}

 BFS(){
    // 1. 변수 초기화
        // 현재 노드 = 루트 노드
        var node = this.root,
            // 순회한 노드의 값 저장
            data = [],
            // 순회한 노드 임시 저장
            queue = [];
        // 2. 루트 노드를 큐에 추가
        // 루트 노드를 큐에 저장
        queue.push(node);

        // 3. 큐가 빌 때까지 순회
        while(queue.length){
            // 큐의 첫번째 노드를 꺼내서 현재 노드로 설정
           node = queue.shift();
           // 현재 노드 값을 data에 추가
           data.push(node.value);
           // 노드의 왼쪽 자식이 존재한다면 큐에 왼쪽 자식을 추가
           if(node.left) queue.push(node.left);
           // 노드의 오른쪽 자식이 존재한다면 큐에 오른쪽 자식을 추가
           if(node.right) queue.push(node.right);
        }
        // 4. 방문한 노드의 값들이 순서대로 저장된 배열 반환
        return data;
    }
```

## 🌱 깊이 우선 탐색(DFS, Depth-first Search)

### 🌿 전위 순회(PreOrder)

![](https://skilled.dev/images/pre-order-traversal.gif)

- 루트 노드에서 왼쪽 노드들을 먼저 순회한 후, 오른쪽 노드 순회
- 부모 노드를 자식 노드보다 먼저 방문

```javascript
 DFSPreOrder(){
        // 1. 결과를 저장할 배열 초기화
        var data = [];
        // 2. 순회 재귀 함수 정의
        function traverse(node){
            // 현재 노드 값 date에 추가
            data.push(node.value);
            // 현재 노드의 왼쪽 자식이 존재한다면
            // 재귀함수 호출하여 왼쪽 자식 순회
            if(node.left) traverse(node.left);
            // 현재 노드의 오른쪽 자식이 존재한다면
            // 재귀함수 호출하여 오른쪽 자식 순회
            if(node.right) traverse(node.right);
        }
        // 3. 루트 노드부터 순회 시작
        traverse(this.root);
        // 4. 방문한 값들이 순서대로 저장된 배열 반환
        return data;
    }
```

### 🌿 후위 순회(PostOrder)

![](https://skilled.dev/images/post-order-traversal.gif)

```javascript
DFSPostOrder(){
        // 1. 결과를 저장할 배열 초기화
        var data = [];
        // 2. 순회 재귀 함수 정의
        function traverse(node){
            // 3. 전위 순회와 반대
            if(node.left) traverse(node.left);
            if(node.right) traverse(node.right);
            data.push(node.value);
        }
        traverse(this.root);
        return data;
    }
```

### 🌿 중위 순회(InOrder)

![](https://skilled.dev/images/in-order-traversal.gif)

```javascript
DFSInOrder(){
        // 1. 결과를 저장할 배열 초기화
        var data = [];
        // 2. 순회 재귀 함수 정의
        function traverse(node){
            // 3. 왼쪽 순회 후 다시 부모 노드로 올라온 후 오른쪽 순회
            if(node.left) traverse(node.left);
            data.push(node.value);
            if(node.right) traverse(node.right);
        }
        traverse(this.root);
        return data;
    }
```

## 🌱 너비 우선 탐색과 깊이 우선 탐색은 언제 쓰는게 좋을까?

- BFS나 DFS의 시간 복잡도는 동일, 차이점은 공간복잡도에 있다.

### 🌿 너비 vs 깊이

![](https://miro.medium.com/v2/resize:fit:1280/1*GT9oSo0agIeIj6nTg3jFEA.gif)

![너비](https://velog.velcdn.com/images%2Fjangws%2Fpost%2Fafd80273-eefe-4761-b751-1734be1fbf5c%2Fex1.jpg)

- BFS를 사용한다면 큐에는 순회하는 모든 노드가 저장된다.
- 하지만 많은 데이터가 큐 저장되기 때문에 공간복잡도가 높아진다.

![넓이](https://velog.velcdn.com/images%2Fjangws%2Fpost%2F1e05c8f5-df74-4fe1-8430-c6122a758e53%2Fex2.jpg)

- DFS를 사용한다면 트리는 수직으로 순회되며 재귀를 이용하기 때문에 공간 복잡도가 낮아진다.

=> 너비가 넓은 트리에서는 DFS를 사용, 깊이가 깊은 트리에서는 BFS를 사용하는 것이 적절하다.
