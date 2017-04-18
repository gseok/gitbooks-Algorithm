### 이진 트리, 이진 탐색 트리, 이진 트리 순회

#### 이진 트리

![](/assets/2Tree.png)

\(그림: [나무위키](https://namu.wiki/w/%ED%8A%B8%EB%A6%AC%28%EA%B7%B8%EB%9E%98%ED%94%84)\)

부모 노드 밑에 자식 노드가 최대 2개밖에 오지 않는, 트리의 가장 간단한 형태. 

두 자식 노드를 보통 왼쪽 자식과 오른쪽 자식으로 구분한다.



#### 이진 탐색 트리

![](/assets/2-search-tree.png)

\(그림: [나무위키](https://namu.wiki/w/%ED%8A%B8%EB%A6%AC%28%EA%B7%B8%EB%9E%98%ED%94%84)\)

이진 트리에 더해서,  부모 노드 기준으로, 정렬이 되어 있는 트리

* 왼쪽 자손 &lt; 부모

* 오른쪽 자손 &gt; 부모

즉 어떤 부모의 왼쪽 자손은, 부모보다 항상 작은 노드로 구성, 반대로 어떤 부모의 오른쪾 자손은 부모보다 항상 큰 형태로 구성



#### 트리 순회

트리의 노드를 방문하는 순서라고 볼 수 있다.

* `전위 순회`\(`Pre-order` traversal\) : 자신, 왼쪽 자손, 오른쪽 자손 순서로 방문

* `중위 순회`\(`In-order` traversal\) : 왼쪽 자손, 자신, 오른쪽 자손 순서로 방문. 

  * `이진 탐색 트리`를 `중위 순회`하면 `정렬된 결과`를 얻을 수 있다.

* `후위 순회`\(`Post-order` traversal\) : 왼쪽 자손, 오른쪽 자손, 자신 순서로 방문하는 순회 방법.

* `레벨 순서` 순회\(`Level-order` traversal\) : `너비 우선 순회`\(Breadth-First traversal&lt;search&gt; \| BFS\)라고도 한다. 노드를 레벨 순서로 방문하는 순회 방법. 

  * 위의 세 가지 방법\(전위, 중위, 후위\)은 [스택](https://namu.wiki/w/%EC%8A%A4%ED%83%9D%28%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%29)을 활용하여 구현할 수 있는 반면 레벨 순서 순회는 [큐](https://namu.wiki/w/%ED%81%90%28%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%29)를 활용해 구현할 수 있다.



순회 예시 \(첫번째 그림을 순회 하였다고 할때\)

* 전위\(pre\) : 2, 7, 2, 6, 5, 11, 5, 9, 4

* 중위\(in\): 2, 7, 5, 6, 11, 2, 5, 4, 9

* 후위\(post\): 2, 5, 11, 6, 7, 4, 9, 5, 2

* 레벨\(bfs\): 2, 7, 5, 2, 6, 9, 5, 11, 4




