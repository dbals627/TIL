# 📚 다익스트라 알고리즘(Dijkstara)

![](https://blog.kakaocdn.net/dn/bN5ZMA/btryG9Yy4GM/xI3QztyQZn6RdQC0n6pvW0/img.png)

- 그래프의 두 정점 사이에 존재하는 최단 경로를 찾는다.

## 📌 가중 그래프

```javascript
class WeightedGraph {
  constructor() {
    this.adjacencyList = {};
  }
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }
  addEdge(vertex1, vertex2, weight) {
    // 정점 사이의 거리를 간선에 추가
    this.adjacencyList[vertex1].push({ node: vertex2, weight });
    this.adjacencyList[vertex2].push({ node: vertex1, weight });
  }
}
```

## 📌 우선순위 큐

```javascript
class PriorityQueue {
  constructor() {
    this.values = [];
  }
  enqueue(val, priority) {
    this.values.push({ val, priority });
    this.sort();
  }
  dequeue() {
    return this.values.shift();
  }
  sort() {
    this.values.sort((a, b) => a.priority - b.priority);
  }
}
```

## 📌 다익스트라 알고리즘 구현(보류..)

```javascript
Dijkstra(start, finish){
        const nodes = new PriorityQueue();
        const distances = {};
        const previous = {};
        let path = [] //to return at end
        let smallest;
        // 초기상태 빌드업
        for(let vertex in this.adjacencyList){
            // 간선이 시작점과 같다면 거리는 0
            if(vertex === start){
                distances[vertex] = 0;
                // 간선과 우선순위(0)을 넣는다.
                nodes.enqueue(vertex, 0);
                // 그렇지 않다면 인피니티
            } else {
                // 간선과 우선순위(무한대)를 넣느다.
                distances[vertex] = Infinity;
                nodes.enqueue(vertex, Infinity);
            }
            // 각 노드마다 previous 값이 널이 되도록 설정
            previous[vertex] = null;
        }
        // 방문할 것이 남아있을 떄
        while(nodes.values.length){
            // 가장 낮은 우선순위를 가진 값을 가진 정점을 제거
            smallest = nodes.dequeue().val;
            if(smallest === finish){
                while(previous[smallest]){
                    path.push(smallest);
                    smallest = previous[smallest];
                }
                break;
            }
            if(smallest || distances[smallest] !== Infinity){
                for(let neighbor in this.adjacencyList[smallest]){
                    let nextNode = this.adjacencyList[smallest][neighbor];
                    let candidate = distances[smallest] + nextNode.weight;
                    let nextNeighbor = nextNode.node;
                    if(candidate < distances[nextNeighbor]){
                        distances[nextNeighbor] = candidate;
                        previous[nextNeighbor] = smallest;
                        nodes.enqueue(nextNeighbor, candidate);
                    }
                }
            }
        }
        return path.concat(smallest).reverse();
    }
```
