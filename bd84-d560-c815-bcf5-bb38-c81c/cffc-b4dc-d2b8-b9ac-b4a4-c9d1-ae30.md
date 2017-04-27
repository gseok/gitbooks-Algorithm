#### 쿼드 트리 뒤집기 풀이

* 문제\(알고스팟\): [https://algospot.com/judge/problem/read/QUADTREE](https://algospot.com/judge/problem/read/QUADTREE)
* 제한시간: 10초

##### 문제에서 주의할 점

* 입력 문자열 max 1000, 그림 크기 max 2^20 x 2^20 이다.

* 입력값이 그림이 아니라. 이미 압축된 문자열이다.

  * x 라는 문자열이 키워드

    * x 문자열 이후에는 4개의 덩어리 트리로 생각하면, 4개의 동일 depth자손\(4개의 노드\)이 생긴다.

    * x 라는 문자열이 아니면, 어떤 덩어리 하나를 의미한다. 트리로 생각하면 하나의 노드를 차지 가능.

* 원하는 답은 상 - 하 를 뒤집는 것, 상하를 뒤집을때 내부의 그림 역시 상하를 뒤집어야 한다.

* 문제에서 text의 순서가 의미 있음.
  * 트리로 봤을때, 동일 자손의 왼쪽 위 노드 0, 오른쪽 위 노드 1, 왼쪽 아래 도느 2, 오른쪽 아래 노드 3
  * 그림으로 생각하면 쉬움

```
처음 주어진 값이
-----
A  B
C  D
-----


이걸 뒤집으면
-----
C  D
A  B
-----


문자열 4개의 덩어리라고 생각하면.
String subStr[] = new String[4];
-----
0  1
2  3
-----


뒤집으면
-----
2  3
0  1
-----
```

문제의 핵심은  'x'  가 나온경우, 'x'가 의미하는 4개의  문자열 덩어리를 찾는 것이다.

여기서 고민해야 할 부분은, 'x'가 나온다음 나와야 하는 4개의 문자열 덩어리 역시 내부적으로 'x' 을 가질 수 있다는 점이다.

즉 'x'이후 4개의 문자열 덩어리의 길이는 가변적이다. 따라서, 'x'이후 4개의 문자열 덩어리중 'x'을 가지고 있으면 또 4개의 문자열 덩어리를 가질 수 있다. 문제 입력값이 이를 순차적으로 표현하고 있다.

결국 문제의 해결 방법은, 위 그림 설명처럼 `0, 1, 2, 3`에 해당하는 문자열을 찾고 이 순서를 뒤집어서 `2, 3, 0, 1` 로 만들어 주면 된다.

> x 이후 가변적인 문자열 처리시 재귀호출이 필수 이다. 경우에 따라서 stack overflow가 발생 할 수 있다.
>
> 따라서, 재귀호출시 stack overflow을 신경써 주어야 한다.



##### 풀이코드

Java로 풀이하였다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

class QuadCalc {
    String treeStr;
    int index;

    public QuadCalc(String treeStr) {
        super();
        this.treeStr = treeStr;
    }

    public String getUpDownReverseWithIndex() {
        char firstChar = treeStr.charAt(index);

        // 'x' 가 아니면 'b' | 'w' 두가지중 하나이다.
        // b, w의 경우 하나의 문자열 덩어리를 의미한다.
        if (firstChar != 'x') {
            index++;
            return firstChar + "";
        } else {
            index++;

            String subStrs[] = new String[4];

            // 문자열 4개의 덩어리를 얻어온다. 0번 덩어리를 확실히 다 얻어와야 1번 덩어리 얻기가 된다.
            // 0번 덩어리 내부에 또 x가 있어서 4개의 덩어리를 얻어도, 재귀호출로 잘 처리된다.
            // 0번 덩어리의 길이는 index로 현재 처리중인 문자열을 지시하고 있다.            
            subStrs[0] = getUpDownReverseWithIndex();
            subStrs[1] = getUpDownReverseWithIndex();
            subStrs[2] = getUpDownReverseWithIndex();
            subStrs[3] = getUpDownReverseWithIndex();
            
            return "x" + subStrs[2] + subStrs[3] + subStrs[0] + subStrs[1];
        }
    }
}

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int C = Integer.parseInt(br.readLine());
        for (int tc = 0; tc < C; tc++) {
            String quadTreeStr = br.readLine();
            QuadCalc calc = new QuadCalc(quadTreeStr);
            bw.write(calc.getUpDownReverseWithIndex() + "\n");
        }

        br.close();
        bw.flush();
        bw.close();
    }

}
```



