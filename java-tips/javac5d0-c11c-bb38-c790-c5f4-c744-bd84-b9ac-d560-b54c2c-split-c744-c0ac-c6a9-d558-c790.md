#### Java에서 문자열을 분리할때, StringTokenizer을 사용하자

Java에서 문자열을 분리할때, 간단하게 `split` 함수를 사용 할 수 있다.  사용하기 편리하다 하지만 속도는 `StringTokenizer` 가 더 빠르다. 따라서, 속도가 중요할 경우에는 `StringToknizer`을 사용하는 것이 좋다.

```java
// 일반적으로 사용
StringTokenizer st = new StringTokenizer("this is a test");
while (st.hasMoreTokens()) {
    System.out.println(st.nextToken());
}

// BufferedReader 와 같이 사용할때
StringTokenizer token = new StringTokenizer(br.readLine());

int val1 = Integer.parseInt(token.nextToken());
int val2 = Integer.parseInt(token.nextToken());
```

split 사용법도 알아두면 좋다. 가장 많이 사용하는 공백자르기 2개는 꼭 기억하자.

```js
// "\\s" 는 공백(space)로 자르기 이다.
String[] result = "this is a test".split("\\s");

// "\\s+" 는 공백(space)가 가변적으로 (스페이스가 하나 이상)인 경우도 정상적으로 잘라주는 표현이다.
for (int x=0; x<result.length; x++)
    System.out.println(result[x]);
```

속도비교 - 참고: [https://github.com/hughperkins/jfastparser](https://github.com/hughperkins/jfastparser)

```
Scanner: 10642 ms
split: 715 ms
StringTokenizer: 544ms
JFastParser: 290ms
```



