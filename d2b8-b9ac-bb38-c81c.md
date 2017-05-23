#### 요새 풀이

* 문제\(알고스팟\): [https://algospot.com/judge/problem/read/NERD2](https://algospot.com/judge/problem/read/NERD2)
* 제한시간: 2초

##### 문제에서 주의할 점

* 참가한 사람 N\(1 &lt;= N &lt;= 50,000\)
* 문제 pi, 라면 qi \(0 &lt;= pi, qi &lt;= 100,000\)
* 테스트 케이스 수 C\(1 &lt;= C &lt;= 50\)

##### 문제에서 주어진 힌트

* 각 사람이 참가 신청을 할 때마다. 그때마다 자격을 체크해서 계산 하라고 되어 있다. 따라서, 전체 참가한사람 5만개 입력을 다 받을 필요 없이 입력을 받음과 동시에 계산 하는 형태를 취하면 된다.
* 새로운 입력값을, 너드 후보자라고 가정하면, 다음 두가지를 체크 할 수 있다.
  * 기존 너드 항목과 비교해서, 새로운 입력값이 너드가 되는지 비교 체크
    * 새로운 입력값이 너드가 아니면, 기존 너드 항목 유지
  * 새로운 입력값이 너드가 된다면, 기존 너드 항목중에서 더이상 너드가 아니게 되는 항목을 찾아서 제거 필요
* 문제에서 비교해야 하는 값이 2가지임\(문제 pi, 라면 qi\) 따라서 일단 한가지 값으로 정렬하고, 나머지 값을 비교하는 형태를 취하면 됨
  * 한가지 값\(pi\)으로 정렬되어 있을때, 새로운 입력값이 오면, 해당 입력값의 한가지값\(pi\)이 이미 작은 부분, 큰 부분 두가지로 나누워서 볼 수 있음.
  * 이후 나머지 값을 비교하면 됨

한가지 값으로 정렬해서, 큰거, 작은거로 나누는걸 생각하면, 이진 트리를 떠올릴 수 있음.



##### 풀이코드

Java로 풀이함

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.Map.Entry;
import java.util.StringTokenizer;
import java.util.TreeMap;

public class Main {

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int C = Integer.parseInt(br.readLine());
        for (int tc = 0; tc < C; tc++) {
            int N = Integer.parseInt(br.readLine());
            int sum = 0;

            // tree는 현재 nerd의 모음
            TreeMap<Integer, Integer> tree = new TreeMap<Integer, Integer>();

            for (int n = 0; n < N; n++) {
                StringTokenizer st = new StringTokenizer(br.readLine());
                int pi = Integer.parseInt(st.nextToken());
                int qi = Integer.parseInt(st.nextToken());

                // pi 가 큰 놈들을 가져와서, qi도 큰지 확인, qi도 큰게 있으면, 새로들어오는 값은 너드가 아님
                Entry<Integer, Integer> higherEntry = tree.higherEntry(pi);

                if (higherEntry == null || (higherEntry != null && higherEntry.getValue() < qi)) {
                    // qi도 큰게 없으면, 일단 새로들어오는 값은 너드임
                    tree.put(pi, qi);

                    // 새로 너드가 들어왔으니, 기존 너드중, 새로들어온 값때문에 너드에서 제외되는거 확인 필요
                    // 너드 제외는, pi 가 작은 놈들을 가져와서, qi도 작은지 확인, qi도 작으면 tree에서 제거
                    Entry<Integer, Integer> lowerEntry = tree.lowerEntry(pi);
                    while (lowerEntry != null) {
                        if (lowerEntry.getValue() < qi) {
                            tree.remove(lowerEntry.getKey());
                        } else {
                            break;
                        }
                        lowerEntry = tree.lowerEntry(pi);
                    }
                }

                // 문제에서 입력이 들어올때마다 동적으로 너드 sum을 계산하라고 했음.
                sum += tree.size();

            }

            bw.write(Integer.toString(sum));
            bw.newLine();
        }

        bw.flush();
        bw.close();
        br.close();
    }

}
```



