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

 

주의 사항

**BFS 형태**

##### 필요한 요소

* 그래프를 표현한 자료구조 \(ArrayList \| Array\)

##### 코드

```java

```

#### 

#### 참고 문서

* 위상정렬 \([http://www.playsw.or.kr/repo/cast/110](http://www.playsw.or.kr/repo/cast/110)\)
* 위상정렬 \([http://pooh-explorer.tistory.com/51](http://pooh-explorer.tistory.com/51)\)
* 위상졍렬 \([https://en.wikipedia.org/wiki/Topological\_sorting](https://en.wikipedia.org/wiki/Topological_sorting)**\)**



