#### 트리 순회 순서 변경 풀이

* 문제\(알고스팟\): [https://algospot.com/judge/problem/read/TRAVERSAL](https://algospot.com/judge/problem/read/TRAVERSAL)
* 제한시간: 1초

##### 문제에서 주의할 점

* 최악의 경우
  * 테스트 케이스 100개
  * 노드수 100개
    * 노드한 한줄로 \(한쪽 방향으로만\) 트리가 구성되었다면 depth 100
    * 테스트케이스 모두 한줄로 된 형태면, depth 100인 걸 100번 돌아야함.
* 전위, 중위 두가지 순회를 주고, 후위 순회를 출력하는 문제
  * depth\(level\) 100인경우, stack overflow가 되지 않도록 주의해야함.
* 트리의 순회 순서만 잘 알고 있으면 별다른 알고리즘 없이 풀이 가능.

##### 풀이코드

Java로 풀이함

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.StringTokenizer;

class TraversalPost {
    int preOrderList[];
    int midOrderList[];
    int rootNodeId;
    int midIndex;

    public TraversalPost(int[] preOrderList, int[] midOrderList) {
        super();
        this.preOrderList = preOrderList;
        this.midOrderList = midOrderList;
        this.rootNodeId = preOrderList[0];

        for (int index = 0; index < this.preOrderList.length; index++) {
            if (this.midOrderList[index] == this.rootNodeId) {
                this.midIndex = index;
                break;
            }
        }
        // binarySearch 음수가 나오는 경우가 있음.
        // this.midIndex = Arrays.binarySearch(this.midOrderList, this.rootNodeId);
        // int testIndex = Arrays.binarySearch(new int[]{10, 479, 150, 838, 441}, 479);
    }

    // using mid
    public int [] getLeftChildNodes(int rootNodeId) {
        if (this.midIndex <= 0) {
            return new int[0];
        }
        return Arrays.copyOf(this.midOrderList, this.midIndex);
    }

    // using mid
    public int [] getRightChildNodes(int rootNodeId) {
        if (this.midIndex + 1 == this.midOrderList.length) {
            return new int[0];
        }
        return Arrays.copyOfRange(this.midOrderList, this.midIndex + 1, this.midOrderList.length);
    }

    public String getPostOrderListString() {
        Object [] answer = this.getPostOrderList().toArray();
        String answerStr = "";
        for (int i = 0; i < answer.length - 1; i++) {
            answerStr += answer[i] + " ";
        }
        answerStr += answer[answer.length - 1];

        return answerStr;
    }
    public ArrayList<Integer> getPostOrderList() {
        ArrayList<Integer> pOrderList = new ArrayList<Integer>();
        int lmidChilds [] = this.getLeftChildNodes(this.rootNodeId);
        int rmidChilds [] = this.getRightChildNodes(this.rootNodeId);

        if (lmidChilds.length == 0 && rmidChilds.length == 0) {
            pOrderList.add(this.rootNodeId);
            return pOrderList;
        }
        if (lmidChilds.length == 1 && rmidChilds.length == 1) {
            pOrderList.add(lmidChilds[0]);
            pOrderList.add(rmidChilds[0]);
            pOrderList.add(this.rootNodeId);
            return pOrderList;
        }
        if (lmidChilds.length == 1 && rmidChilds.length == 0) {
            pOrderList.add(lmidChilds[0]);
            pOrderList.add(this.rootNodeId);
            return pOrderList;
        }
        if (lmidChilds.length == 0 && rmidChilds.length == 1) {
            pOrderList.add(rmidChilds[0]);
            pOrderList.add(this.rootNodeId);
            return pOrderList;
        }

        // left string + right string + this
        int [] subPreOrder;
        if (lmidChilds.length > 0) {
            subPreOrder = Arrays.copyOfRange(this.preOrderList, 1, 1 + lmidChilds.length);
            pOrderList.addAll(new TraversalPost(subPreOrder, lmidChilds).getPostOrderList());
        }

        if (rmidChilds.length > 0) {
            subPreOrder = Arrays.copyOfRange(
                    this.preOrderList, 1 + lmidChilds.length, this.preOrderList.length);
            pOrderList.addAll(new TraversalPost(subPreOrder, rmidChilds).getPostOrderList());
        }
        pOrderList.add(this.rootNodeId);

        return pOrderList;
    }
}

public class Traversal {
    public static void main(String[] args) throws Exception {
        InputStreamReader fr = new InputStreamReader(System.in);
        BufferedReader br = new BufferedReader(fr);

        int testCase = Integer.parseInt(br.readLine());

        for (int tc = 0; tc < testCase; tc++) {
            int nodeNum = Integer.parseInt(br.readLine());
            String preOrder = br.readLine();
            String midOrder = br.readLine();

            StringTokenizer pst = new StringTokenizer(preOrder);
            StringTokenizer mst = new StringTokenizer(midOrder);

            int preOrderList[] = new int[nodeNum];
            int midOrderList[] = new int[nodeNum];

            for (int n = 0; n < nodeNum; n++) {
                preOrderList[n] = Integer.parseInt(pst.nextToken());
                midOrderList[n] = Integer.parseInt(mst.nextToken());
            }

            String answer = new TraversalPost(preOrderList, midOrderList).getPostOrderListString();
            System.out.println(answer);
        }
        br.close();
    }
}
```



