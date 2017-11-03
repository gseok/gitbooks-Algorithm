프림 알고리즘

```java
    public static boolean connected [];
    public static ArrayList<ArrayList<Edge>> nodes;

    static class Edge implements Comparable<Edge> {
        int from, to;
        double length;

        public Edge(int from, int to, double length) {
            super();
            this.from = from;
            this.to = to;
            this.length = length;
        }
        @Override
        public int compareTo(Edge other) {
            return Double.compare(this.length, other.length);
        }
    }

    public static double getPrimsMinSum() {
        double sum = 0;
        // get start point's candidate edges, start is 0 node
        ArrayList<Edge> edges = nodes.get(0);
        PriorityQueue<Edge> pq = new PriorityQueue<Edge>();
        pq.addAll(edges);

        // prims
        while (!pq.isEmpty()) {
            Edge edge = pq.poll();

            // if already from, to connected then bypass
            if (connected[edge.from] && connected[edge.to]) {
                continue;
            }

            // edge connect
            connected[edge.from] = true;
            connected[edge.to] = true;
            sum += edge.length;

            // next candidate edges
            ArrayList<Edge> nextEdges = nodes.get(edge.to); // edge.to is new 'from'
            pq.addAll(nextEdges);
        }
        return sum;
    }
```



쿠르스칼 알고리즘

```java

    // 쿠르스칼에서는, circle을 판단하기위한 부분이 필요하다.
    public static int root[] = new int[TotalNodeNumber];

    public static void initRoot() {
        // 처음에는 모든 node의 부모는 자기 자신이니깐 -1로 설정
        Arrays.fill(root, -1);
    }

    public static int find(int index) {
        // 0보다 작으면 최상위 부모임.
        if (roots[index] < 0) {
            return index;
        }
     
        // 아니면 부모의 indxe을 가지고 있으니깐 찾아라~
        return find(roots[index]);
    }

    // 서로 다른 두개의 그룹을 합치는 방법.
    // 두 그룹의 최상위 부모를 알아온뒤, 한쪽 최상위 부모를 다른쪽 부모에다가 달면 됨.
    public static void union(int a, int b) {
        int root1 = find(a);
        int root2 = find(b);
      
        // 이미 같은 그룹이라면
        if (root1 == root2) {
            return;
        }
      
        // root1과 root2를 결합하고, root2의 부모를 roo1로 설정
        roots[root1] += roots[root2];
        roots[root2] = root1;
      
    }

    public static ArrayList<Edge> edges;
    public static double getKrusKalMinSum() {
        double sum = 0;
        int edgeCount = 0;
        
        // 가능한 모든 edges을 queue에 넣는다. queue을 쓰지 않는 경우, sort을 이용해도 무관하다.
        PriorityQueue<Edge> pq = new PriorityQueue<Edge>();
        pq.addAll(edges);
        
        while(!pq.isEmpty()) {
            // 길이가 가장 짧은 (최적의) edge을 가져온다.
            Edge edge = pq.poll();
                 
            int aRootIndex = find(edge.from);
            int bRootIndex= find(edge.to);

            // 시작점, 도착점의 최상위 부모를 가져온다.                 
            if (aRootIndex == bRootIndex) {
                // 만약 최상위 부모가 같으면, 현재 이미 연결선이 있는 상태에서 circle이 발생하는 것이다.
                // 따라서 최상위 부모가 같으면 패스..
                continue;
            } else {
                sum += edge.length;
                ++edgeCount; // edge 카운팅을 하는 이유는, N-1개의 선이 연결되면 트리가 완성! 이걸 체크하기위해서.
                     
                if (edgeCount + 1 == N) {
                    break;
                }
                
                // edge가 연결되었으니, edge연결을 관리(circle판단)하는 root배열을 갱신!
                union(aRootIndex, bRootIndex);
            }
        }
    }
```







