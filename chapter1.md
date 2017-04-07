# Tips

실제 문제를 풀때 도움이 되는 팁을 모아두었다

#### 문제를 풀때 기본 form을 하나 기억하자

문제를 풀때, 아래와 같은 기본 form을 하나 외워서 사용하면 좋다.

* 속도 체크
* 문제 data을 읽어오는건 BufferedReader로

```java
public class MyClass {
    public static void main(String[] args) throws Exception {
        // 속도체크
        long start = System.currentTimeMillis();

        // 문제 읽어오기
        FileInputStream in = new FileInputStream("inputs/sample.txt");
        BufferedReader  bf = new BufferedReader(new InputStreamReader(in));
        int intValue = Integer.parseInt(bf.readLine());
        String stringValue = bf.readLine();

        // 리소스 반납
        bf.close();
        
        // 속도체크 출력
        System.out.println(System.currentTimeMillis() - start + "ms");
    }
}
```



#### 파일에서 문제를 읽어올때는 BufferedReader을 사용하자

java에서 scanner을 사용하여 문제 파일을 읽을 수 도 있지만, scanner보다는 BufferedReader을 사용하면 성능의 향상이 있다.

BufferedReader는 버퍼에 stream data을 한번에 읽어와서 동작하기 때문이다.

```java
public class MyClass {
    public static void main(String[] args) throws Exception {

        FileInputStream in = new FileInputStream("inputs/sample.txt");
        BufferedReader  bf = new BufferedReader(new InputStreamReader(in));
        int intValue = Integer.parseInt(bf.readLine());
        String stringValue = bf.readLine();

        bf.close();
    }
}
```

BufferedReader에서 값들을 읽어오는 방법은 간단하다. 다음을 기억하자

1. 원본이 되는 파일에서 값을 읽어와야 하니, FileInputStream을 이용해서 일단 파일에서 값을 읽어온다
2. 읽어온 stream을 Reader로 감싼다
   1. 읽어온 inputStream을 BufferedReader을 상용해서 읽어와야 하는데, BufferedReder는 Reader 클래스를 param으로 주어야 한다. 따라서  stream을 InputStreamReader로 변경해준다
3. BufferedReader에 Reader을 넣어서 BufferedReader클래스를 생성한다
4. BufferedReader에서 제공하는 함수를 사용해서 실제 값을 읽어 사용한다
   1. readLine\(\)의 경우 바로 읽으면 String을 리턴한다. int 값으로 사용하고 싶은경우 Integer.parseInt로 변환을 한다
5. 사용이 끝났으면 BufferedReader을 close해준다



#### 동적계획법에서 Memo사용시 -1, 0, 1 대신 명시적인 TRUE, FALSE을 사용하자

동적계획법을 사용하는경우, 속도 향상을 위해 memo을 하게 된다. 이 경우 일반적으로 배열을 사용하게 된다.

보통 배열을 사용해서 memo을 하는경우, -1 \(메모안됨\), 0 \(실패\), 1\(성공\) 와 같이 내부적으로 특정 형태를 가정하면서 진행하게 된다.

이러한 방법을 사용하는것 보다는 아래와 같이 명시적인 값을 만들어서 사용하는 것이 좋다

```java
public class MyClass {
    // 아무값이나 유일하게 사용할 수 있으면 됨
    public static int TRUE  = 3;
    public static int FALSE = -9;
    public static int memo[][] = new int[100][100];

    public static void main(String[] args) throws Exception {
         // 사용, i, j index는 어딘가에서 잘 주었다고 했을때..
         if (memo[i][j] == TRUE) {
             // true 처리
         } else if (memo[i][j] == FALSE) {
             // false 처리
         } 

         // 굳이 배열 초기화를 하지 않아도 기본 생성된 초기값을 이용 할 수 있음
         if (memo[i][j] == 0) { // memo가 아직 안되어 있음. 0은 new int시 자동 초기화된 값
             // memo가 안되었을때 처리
         }
    }
}
```

장점

* 배열 초기화를 할 필요 없음
* -1, 0, 1 와 같은 값이 아니어서, 코딩중 실수를 줄여준다. 명시적인 TRUE, FALSE로 구현 가능



