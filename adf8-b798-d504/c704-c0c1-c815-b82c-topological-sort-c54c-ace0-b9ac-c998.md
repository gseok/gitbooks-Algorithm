### 위상정렬 \(Topological Sort\) 알고리즘

#### 이론 핵심 요약

* 노드를 순서대로 나열하는 알고리즘
  * 방향 그래프에서, 정점을 순서에 맞게 나열하는 것 \(게임의 스킬 트리를 생각하면됨\)
  * **순환\(사이클\)이 있으면 안됨**
  * 어떤 작업에 `단계`가 \(순서가\) 반드시 지켜져야 하는 겨우 이를 `나열`하는데 사용
  * 복잡도 O\(V + E\)
    * DFS나 BFS 형태로 만들 수 있다.
    * 인접 행렬 사용시 O\(V^2\)까지 나올 수도 있음.

#### 구현 핵심 요약

Java로 설명한다.

**DFS 형태**

##### 필요한 요소

* 그래프를 표현한 자료구조 \(ArrayList \| Array\) - e.g\) `int [][] graph`
* 방문 체크 자료구조 \(int\[\] visited\) - e.g\) `int [] visited`
* 위상으로 정렬된 노드를 저장할 자료 구조 \(ArrayList, Array\) - e.g\) `ArrayList<Integer> sortedGraph`

##### 코드

```java
public static int[] visited;
public static int[][] graph;
public static ArrayList<Integer> sortedGraph;

public void dfs(int node) {
    visited[node] = 1;

    for (int i = 0; i < graph[node].length; i++) {
        if (graph[node][i] == 1 && visited[i] == 0) {
            dfs(i);
        }
    }

    // save topological sort
    sortedGraph.add(node);
}

public void topologicalSortDFS() {
    for (int i = 0; i < graph.length; i++) {
        if (visited[i] == 0) {
            dfs(i);
        }
    }
}
```

> 주의 사항

* sortedGraph 에 저장하는 node \(순서\)는 경우에 따라 reverse 해서 사용해야 할 수 도 있음.
  * 아니면 LinkedList 을 사용해서, addFirst 로 항상 맨 앞에 추가하는 형태를 사용해서 reverse없이 바로 원하는 순서를 얻을 수 있다.

**BFS 형태**

##### 필요한 요소

* 그래프를 표현한 자료구조 \(ArrayList \| Array\)
* 다음노드\(연결된 노드\) 방문 자료구조 \(Queue\)
* 차수\(위상 - 간선\) 저장용 자료구조 \(Array\) - `e.g) int[] indegree;`

##### 코드

```java
public static int[][] graph;
public static int[] indegree;
public static ArrayList<Integer> sortedGraph;


public void bfs() {
        Queue<Integer> q = new LinkedList<Integer>();

        for (int i = 0; i < indegree.length; i++) {
            if (indegree[i] == 0) {
                indegree[i]--; // 큐에 다시 넣지 않도록 확실히 -값으로 만들어버림
                sortedGraph.add(i);
                q.add(i);
            }
        }

        while (!q.isEmpty()) {
            // 큐에 있는 노드는 차수 0인노드, 따라서, 해당 노드가 없어졌다고 보고, 
            // 해당 노드와 연결된 간선과, 연결된 노드의 차수를 줄여줌.
            int node = q.poll();

            for (int n = 0; n < graph.length; n++) {
                if (graph[node][n] == 1) { // 연결된 노드면
                    graph[node][n] = 0; // 연결 제거
                    indegree[n]--; // 연결된 노드의 차수 제거
                }

                if (indegree[n] == 0) {
                    indegree[n]--;
                    sortedGraph.add(n);
                    q.add(n);
                }
            }
        }
    }

public void topologicalSortBFS() {
    // create indegree, 차수만들기
    for (int i = 0; i < graph.length; i++) {
        for (int j = 0; j < graph.length; j++) {
            if (graph[i][j] == 1) {
                indegree[j]++; // i -> j 로 가는 간선이 있다는 말은 j입장에서는 j에 대한 위상이 하나 존재한다는 뜻.
            }
        }
    }

    bfs();
}
```

* `topological sort`의 기본 개념과 동일한 형태로 구현됨
  * 각 노드별 차수\(위상\)을 저장
  * `차수`\(위상\) 이 `0` 인 노드를 `순서대로 저장`, 그리고 해당 `노드를 큐에 저장`
  * 차수 0인 노드들이 가리키고 있던\(연결된\) 노드의 차수를 줄여줘야함. \(큐에서 꺼내와서 하나씩\)
  * 연결된 노드의 차수를 줄였을때, 해당 노드의 `차수가 0` 이면,  `순서대로 저장` & `큐에 저장`

#### 참고 문서

* 위상정렬 \([http://www.playsw.or.kr/repo/cast/110](http://www.playsw.or.kr/repo/cast/110)\)
* 위상정렬 \([http://pooh-explorer.tistory.com/51](http://pooh-explorer.tistory.com/51)\)
* 위상졍렬 \([https://en.wikipedia.org/wiki/Topological\_sorting](https://en.wikipedia.org/wiki/Topological_sorting)**\)**



