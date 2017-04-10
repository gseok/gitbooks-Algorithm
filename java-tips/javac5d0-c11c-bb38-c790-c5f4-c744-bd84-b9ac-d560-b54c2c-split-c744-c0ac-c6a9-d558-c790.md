#### Java에서 문자열을 분리할때, split을 사용하자

Java에서 문자열을 분리할때, 간단하게 `split` 함수를 사용 할 수 있다. 기존에는 `StringTokenizer`을 사용했다. 하지만 Java 공식 문서에서 `StringTokenizer`is a legacy class 라고 되어 있다.

즉 Java에서 `StringTokenizer` 보다는 `split`을 쓰도록 권유 하고 있다.

아래 링크에는 `split`와 `StringTokenizer`의 속도를 비교한 부분이 있다. 확실히 split이 좀 더 빠르게 동작한다.

속도비교 참고: [http://stackoverflow.com/questions/5965767/performance-of-stringtokenizer-class-vs-split-method-in-java](http://stackoverflow.com/questions/5965767/performance-of-stringtokenizer-class-vs-split-method-in-java)

JAVA API 문서 참고: [https://docs.oracle.com/javase/8/docs/api/java/util/StringTokenizer.html](https://docs.oracle.com/javase/8/docs/api/java/util/StringTokenizer.html)

split 코드 사용법, 가장 많이 사용하는 공백자르기 2개는 꼭 기억하자

```js
// "\\s" 는 공백(space)로 자르기 이다.
String[] result = "this is a test".split("\\s");

// "\\s+" 는 공백(space)가 가변적으로 (스페이스가 하나 이상)인 경우도 정상적으로 잘라주는 표현이다.
for (int x=0; x<result.length; x++)
    System.out.println(result[x]);
```



