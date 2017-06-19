#### 요새 풀이

* 문제\(알고스팟\): [https://algospot.com/judge/problem/read/SORTGAME](https://algospot.com/judge/problem/read/SORTGAME)
* 제한시간: 2초

##### 문제에서 주의할 점

* 수열의 길이 N \(1 &lt;= N &lt;= 8\)
* 테스트 케이스 수 C\(1 &lt;= C &lt;= 1000\)
* 수열의 수는 중복 되지 않음
* 모든 수는 1 ~ 1,000,000 \(백만\) 사이의 수이다
* 구하고 싶은건, 최소 뒤집기 횟수



##### 문제에서 주어진 힌트

* 기본적으로 N개의 원소가 있을때, 해당 원소를 가지고 만들 수 있는 배열의 가지수는 n! 이다.
  * A, B, C 원소가 있다면, \(A, B, C\), \(A, C, B\), \(B, A, C\), \(B, C, A\), \(C, A, B\), \(C, B, A\)
  * A, B, C, D 라면 위 3개 있을때 경우의 수 6개가 4가지 경우로 존재\(A가 맨앞, B가 맨압, C가 맨앞, D가 맨앞\)
    * 따라서 4! = 3! \* 4 = 24
* 문제에서 MAX 8 개의 원소가 있을 수 있다고 \(수열의 길이8\) 했음 따라서
  * 1 \* 2 \* 3 \* 4 \* 5 \* 6 \* 7 \* 8 = 40320의 경우의 수가 있음
  * 1문제 최악의 경우 4만개의 경우의 수를 찾아야 하는데 TC수가 1000개임
  * 따라서 4만 \* 1000 = 4억 \(약 4초\)가 걸린다. 
* 만약 수열이 주어졌을때, 매번, 모든 경우의 수를 찾으면 시간 초과

**두가지 해결 포인트**

* 숫자가 다르지만 결국 상대적인 크기는 같다.
  * {30, 40, 10, 20} 은 {3, 4, 1, 2} 와 동일
  * {321, 430, 134, 245} 4개의 수가 주어졌다면, 역시 {3, 4, 1, 2} 와 동일하게 볼 수 있음.
  * 따라서 문제 입력값을 {0, 1, 2, 3, 4, 5, 6, 7, 8} 0 ~ 8 인 수로 normalize하고, 실제 0 ~ 8 인 수의 정답과 동일
* 수를 뒤집는 최소 횟수를 구하는 문제인데, 기본적으로 양방향으로 생각 가능
  * 즉, {0, 1, 2, 3}을 한번 뒤집어서 {1, 0, 2, 3}을 구하는 것과
  * 처음 {1, 0, 2, 3}을 한번 뒤집에서 {0, 1, 2, 3}을 구하는 것과 횟수가 동일함
  * 따라서 이렇게 생각 할 수 있다.
    * {0, 1, 2, 3} 을 뒤집어서 나올수 있는 모든 경우의 수를 다 구해둠 \(구하면서, 뒤집는 횟수와, 뒤집었을때의 수열 모양을 저장\)
    * 입력으로 어떤 수가 주어지면, 위에 미리 구해둔 경우의 수와 바로 매칭해서 몇번 0 ~ N을 몇번 뒤집었을때, 입력으로 주어진 수가 나오는지 확인하면, 그게 바로 답이됨

두가지 해결 포인트를 떠올리고, 미리 계산 \(동적계획법 메모\)을 하는게 핵심인 문제이다.

주의: 아래 풀이 코드 동일 로직에서, 자료구조 Vector 을 메모로 사용하냐, ArrayList을 사용하냐에 따라 시간이 2배 차이 난다. 이유는 Vector의 경우, java 내부적으로 sync lock하는 형태로 구동되기때문에 느리게 동작 할 수 있다. 따라서 자료구조 사용시 ArrayList 사용을 권장한다.



##### 풀이코드

Java로 풀이함

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.Map;
import java.util.Queue;
import java.util.StringTokenizer;
import java.util.Vector;

public class Main {
    public static int numberCnt;
    public static Vector<Integer> num;
    public static Map<ArrayList<Integer>, Integer> memo = new HashMap<ArrayList<Integer>, Integer>();

    @SuppressWarnings("unchecked")
    public static void preCalc(int preCalcNum) {
        // 어차피 비율로 계산되니, 이미 정렬된 형태의 루트 노드를 하나 만듬
        ArrayList<Integer> perm = new ArrayList<Integer>();
        for (int i = 0; i < preCalcNum; i++) {
            perm.add(i);
        }

        // bfs형태로 그래프를 생성하면서 동시에 dist을 기록(memo)해둔다
        Queue<ArrayList<Integer>> q = new LinkedList<ArrayList<Integer>>();

        // 큐의 시작은 위에서 만든 루트 노드
        memo.put(perm, 0);
        q.add(perm);

        while (!q.isEmpty()) {
            // 큐에서 하나 꺼네욤
            ArrayList<Integer> here = q.poll();

            // 현재 위치까지의 거리
            int cost = memo.get(here);

            // 실제로 뒤집기를 해본다
            for (int i = 0; i < preCalcNum; i++) {
                for (int j = i + 2; j <= preCalcNum; j++) {
                    // 사실상 뒤집어져서 나온 값은, 그래프에서 새로운 노드와 마찬가지임
                    // 우리 그래프의 하나의 노드는 vector니깐!
                    ArrayList<Integer> clone = (ArrayList<Integer>) here.clone();
                    Collections.reverse(clone.subList(i, j));

                    if (memo.get(clone) == null) {
                        memo.put(clone, cost + 1);
                        q.add(clone);
                    }
                }
            }
        }

        return;
    }

    public static String getSolution() {
        // 문제로 주어지는 수열을, 0 ~ n 형태의 수열로 치환한다.
        Vector<Integer> newVector = new Vector<Integer>(numberCnt);
        for (int i = 0; i < numberCnt; i++) {
            int smaller = 0;
            for (int j = 0; j < numberCnt; j++) {
                if (num.get(j) < num.get(i)) {
                    smaller++;
                }
            }
            newVector.add(i, smaller);
        }

        return String.valueOf(memo.get(newVector));
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        
        int tc = Integer.parseInt(br.readLine());

        for (int i = 1; i < 9; i++) {
            preCalc(i);
        }

        for (int c = 0; c < tc; c++) {
            numberCnt = Integer.parseInt(br.readLine());
            num = new Vector<Integer>();

            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int n = 0; n < numberCnt; n++) {
                int val = Integer.parseInt(st.nextToken());
                num.add(val);
            }
            
            bw.write(getSolution());
            bw.newLine();
        }

        br.close();
        bw.flush();
        bw.close();
    }
}
```



