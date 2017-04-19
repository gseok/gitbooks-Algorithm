### BFS \(Breath First Search\) 너비 우선 탐색

#### 이론 핵심 요약

![](/assets/dfs-bfs.gif)

[그림 출처](https://namu.wiki/w/BFS)

* 탐색 알고리즘

  * `그래프`나, `트리`에서 존재하는 `모든 노드를 방문`\(탐색\) 하는 방법의 하나
  * BFS는 한 루트에서, 해당 노드에 연결된 모든 노드를 먼저 탐색하고, 그다음, 다음 depth로 넘어가서 탐색하는 형태
  * `최단 경로`를 알 수 있음.
    * 어느 한 점에서, 갈 수 있는 모든 노드를 탐색하는 스타일이기 때문
  * 복잡도 O\(V^2\)

    * 최악의 경우 V^2의 복잡도.

    * 그래프의 표현을 인접 행렬 O\(V^2\) 대신, 인접 리스트 사용시 O\(V + E\)로 줄일 수 있다.

      * V: 정점\(Vector\), E: 간선\(Edge\)

#### 구현 핵심 요약

Java로 설명한다.

##### 필요한 요소 {#필요한-요소}

* 그래프를 표현한 자료구조 \(ArrayList \| Array \[\]\[\]\)
* **방문 정보를 기록**하기위한 테이블 \(int visted\[\]\)
* 출발 노드와, 출발 노드에서 갈 수 있는 노드를 저장하기 위한 큐, \(Queue&lt;Object&gt; queue\)

##### 코드 {#코드}

###### 인접행렬

```java
public boolean[] visited;
public int[][] graph;

public void bfs(int node) {
    Queue<Integer> queue = new <Integer>LinkedList();

    queue.add(node);
    visited[node] = true;

    while (!queue.isEmpty()) {
        int visitNode = q.poll();

        for (int i = 0; i < graph[visitNode].length; i++) {
            if (graph[visitNode][i] == 1 && visited[i] == false) {
                queue.add(i);
                visited[i] = true;
            }
        }
    }
}

public void bfsAll() {
    for (int i = 0; i < graph.length; i++) {
        if (visited[i] == false) {
            bfs(i);
        }
    }
}
```

##### 인접리스트

```java
public boolean[] visited;
public ArrayList<ArrayList<Integer>> graph;

public void bfs(int node) {
    Queue<Integer> queue = new <Integer>LinkedList();

    queue.add(node);
    visited[node] = true;

    while (!queue.isEmpty()) {
        int visitNode = q.poll();

        for (int connectedNode : graph.get(visitNode)) {
            if (visited[connectedNode] == false) {
                queue.add(connectedNode);
                visited[connectedNode] = true;
            }
        }

        // 위 for문은 아래와 같은 의미이다.
        ArrayList<Integer> connectableNodeList = graph.get(visitNode);
        for (int i = 0; i < connectableNodeList.size(); i++) {
            int connectedNode = connectableNodeList.get(i);
            if (visited[connectedNode] == false) {
                queue.add(connectedNode);
                visited[connectedNode] = true;
            }
        }
    }
}

public void bfsAll() {
    for (int i = 0; i < graph.size(); i++) {
        if (visited[i] == false) {
            bfs(i);
        }
    }
}
```

###### 의사코드

```
// 너비 우선

BFS (G, s) // 그래프 G와 출발 코드 s
    Q <= 0; // Q에는 현재 아무 노드들도 들어가 있지 않는 상태 (초기)
    Enqueue(Q, s); // Q에 출발 코드 S를 input
    while Q != 0 do // Q에 더이상 아무 노드들이 없을때까지 루프
        u <= Dequeue(Q) // Q에서 노드를 빼서
        for each v adjacent to u do // u의 인접한 노드 v들을 순회한다.
            if v is unvisited then // u의 인접한 노드 v가 방문하지 않았던 노드라면,
                mark v as visited; // v를 방문했다는 표시를 하고
                Enqueue(Q, v); // Q에 노드 v를 집어 넣는다.
            end.
        end.
    end.
```



