# 📚 그래프(Graphs)

![](https://velog.velcdn.com/images%2Fjangws%2Fpost%2Ffdeffdd8-59a4-44cd-9ca3-2649ee8997dc%2Fgraph1.jpg)

- 그래프는 유한하고 변할 수 있는 꼭지점이나 노드나 점들의 집합으로 구성된 데이터 구조
- 꼭지점들의 집합에 순서가 없는 경우는 무방향 그래프, 순서가 있는 경우는 유방향 그래프라고 한다.
- 노드나 노드들의 연결을 모은 것이다.
- 트리나 연결 리스트 같은 수준의 패턴은 없다.

> 그래프 활용 사례
>
> - 소셜 네트워크
> - 지도 길 찾기
> - 라우팅 알고리즘
> - 추천 알고리즘

## 📌 그래프 용어

- **Vertex(정점)** : 노드
- **Edge(간선)** : 노드와 노드사이를 연결하는 선
- **Weighted/Unweighted(가중/비가중)** : 노드 사이의 거리

  <img src="images/가중 그래프.png">

  - **Weighted Graph** : 간선에 값을 부여하면 가중 그래프가 된다(ex. 최단경로 계산)

- **Directed/Undirected(방향/무방향)**

  <img src="images/단방향 그래프.png">

  - **Directed Graph** : 단방향 연결, 보통 방향을 의미하는 화살표로 표현(sns 팔로잉)

  <img src="images/무방향 그래프.png">

  - **Undirected Graph** : 양방향 연결(sns 맞팔)

## 📌 그래프 정렬 : 인접 행렬(Adjanceny Matrix)

<img src="images/인접 행렬.png">

- 행렬은 이차원 구조로 보통 중첩 행렬로 표현한다. 기본적으로 행과 열에 맞춰서 정보를 저장한다.
- 남는 공간이 많기 때문에 더 많은 공간을 차지한다. 퍼져있는 데이터를 다룰 때는 별로 좋지 않다. 하지만 데이터가 거의 차 있는 경우에는 효율적이다.
- 모든 간선을 순회하는 것은 더 느리다.

## 📌 그래프 정렬 : 인접 리스트(Adjanceny List)

<img src="images/인접 리스트.png">

- 노드와 선 관계를 해시 테이블을 이용하여 저장할 수 있다.
- 간선이 많지 않고 퍼져있는 그래프에 대해서는 실제로 있는 연결만 저장하기 때문에 인접 행렬보다 더 적은 공간을 차지한다.
- 모든 간선을 순회하는 것은 더 빠르지만 특정 간선을 확인하는 것은 더 느리다.

### 정점 추가 - addVertex

```javascript
class Graph {
  constructor() {
    this.adjacencyList = {};
  }
  // 입력받은 정점이 인접리스트에 없으면 key에 value를 빈 배열로 설정(빈 노드 생성)
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }
}
```

### 간선 추가 - addEdge

```javascript
 addEdge(v1,v2){
    // 노드값 두개를 서로의 배열에 push
        this.adjacencyList[v1].push(v2);
        this.adjacencyList[v2].push(v1);
    }
```

### 간선 제거 - removeEdge

```javascript
// 제거하려는 간선으로 연결된 두개의 정점을 입력받아
// 인접리스트의 각각의 정점들의 value에서 상대 정점만 제외하여 반환
removeEdge(vertex1, vertex2) {
  this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(v => v !== vertex2);
  this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(v => v !== vertex1);
}
```

### 정점 제거 - removeVertex

```javascript
// 정점과 연결된 모든 간선을 삭제하고 정점도 삭제해야 한다.
removeVertex(vertex){
        // 주어진 정점과 연결된 간선이 존제하는 경우 반복
        while(this.adjacencyList[vertex].length){
            // 해당 정점과 연결된 간선들을 모두 삭제한다.
            const adjacentVertex = this.adjacencyList[vertex].pop();
            this.removeEdge(vertex, adjacentVertex);
        }
        // 정점도 삭제한다.
        delete this.adjacencyList[vertex]
    }
```

# 📚 그래프 순회

![](https://blog.kakaocdn.net/dn/oKP5E/btqxwEjHwHW/D6PehDsmMGS2jK5Zkp8rI1/img.png)

- 방문, 업데이트, 확인, 출력 등을 그래프의 모든 정점에 대해 수행한다.
- 그래프는 루트가 없기 때문에 그래프를 순회하는 코드를 짤 때 시작점을 정해줘야 한다.

> 그래프 활용 사례
>
> - 네트워킹
> - 웹 크롤링
> - 최단 거리 찾기

## 📌 DFS

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcbFJoH%2FbtrgMsTPnjO%2FNu6flCQiYVsX2vIKXJGjE1%2Fimg.png)

### 재귀형

```javascript
depthFirstRecursive(start){
        // 최종 결과를 반환할 리스트 만들기
        const result = [];
        // 이미 방문한 정점들을 저장할 객체 만들기
        const visited = {};
        // 그래프의 인접 리스트를 가져옴
        const adjacencyList = this.adjacencyList;

        // 재귀 함수를 사용하여 DFS 실행
        (function dfs(vertex){
            // 정점이 있다면 함수 종료
            if(!vertex) return null;
            // 정점을 방문했음을 표시하고 배열에 추가
            visited[vertex] = true;
            result.push(vertex);
            // 현재 정점과 연결된 각 노드에 대해 반복
            adjacencyList[vertex].forEach(neighbor => {
                // 노드가 방문되지 않았다면 재귀함수 호출
                if(!visited[neighbor]){
                    return dfs(neighbor)
                }
            });
        })(start);
        // 결과 반환
        return result;

        //          A
        //        /   \
        //       B     C
        //       |     |
        //       D --- E
        //        \   /
        //          F

        // [ 'A', 'B', 'D', 'E', 'C', 'F' ]
    }
```

### 순환형

- 스택(LIFO, 후입선출)을 사용한다.

```javascript
 depthFirstIterative(start){
        // 스택 초기화하고 시작 정점을 추가
        const stack = [start];
        // 결과 저장할 배열 초기화
        const result = [];
        // 방문한 정점을 저장할 객체 초기화
        const visited = {};
        // 현재 정점을 나타내는 변수 초기화
        let currentVertex;
        // start 노드는 방문했음을 기록
        visited[start] = true;

        // 스택이 비어있지 않는다면 계속 반복
        while(stack.length){
            // 스택에서 가장 최근에 들어온 노드를 꺼내어(pop) result 변수에 추가
            currentVertex = stack.pop();
            result.push(currentVertex);

            // 현재 정점과 연결된 각 이웃에 대해
            this.adjacencyList[currentVertex].forEach(neighbor => {
                // 이웃이 아직 방문되지 않았다면
               if(!visited[neighbor]){
                    // 이웃을 방문했음을 표시하고 스택에 추가
                    // 스택에 이제 start 정점은 없고 그 다음에 방문한 정점 하나가 남는다.
                    // 다시 while문 반복
                   visited[neighbor] = true;
                   stack.push(neighbor)
               }
            });
        }
        // 결과 반환
        return result;

        //          A
        //        /   \
        //       B     C
        //       |     |
        //       D --- E
        //        \   /
        //          F

        // [ 'A', 'C', 'E', 'F', 'D', 'B' ]
    }
```

## 📌 BFS

- 같은 레벨(높이)에 있는 이웃 정점들을 먼저 방문하는 알고리즘이다.
- 큐(FIFO, 선입선출)를 사용한다.

![]()

```javascript
 breadthFirst(start){
        // 큐를 초기화하고 시작 정점 추가
        const queue = [start];
        // 최종 결과를 반환할 리스트 만들기
        const result = [];
        // 이미 방문한 정점들을 저장할 객체 만들기
        const visited = {};
        // 현재 정점을 나타내는 변수 초기화
        let currentVertex;
        // start 노드는 방문했음을 기록
        visited[start] = true;

        // 큐가 비어있지 않는다면 계속 반복
        while(queue.length){
            // 큐에서 처음 정점을 꺼내서 현재 정점으로 설정
            currentVertex = queue.shift();
            // 결과 배열에 현재 정점 추가
            result.push(currentVertex);

            // 현재 정점과 연결된 각 이웃에 대해
            this.adjacencyList[currentVertex].forEach(neighbor => {
                // 이웃이 아직 방문되지 않았다면
                if(!visited[neighbor]){
                    // 이웃을 방문했음을 표시하고 큐에 추가
                    // 큐에 이제 start 정점은 없고 그 다음에 방문한 정점 하나가 남는다.
                    // 다시 while문 반복
                    visited[neighbor] = true;
                    queue.push(neighbor);
                }
            });
        }
        return result;

        //          A
        //        /   \
        //       B     C
        //       |     |
        //       D --- E
        //        \   /
        //          F

        // QUEUE: []
        // result : [A, B, C, D, E, F]
    }
```
