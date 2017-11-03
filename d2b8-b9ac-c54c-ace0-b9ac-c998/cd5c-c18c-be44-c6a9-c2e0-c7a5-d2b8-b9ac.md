

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



