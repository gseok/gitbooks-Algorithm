#### Java char 초기화와 String 초기화를 구별하자

Java에서 char 초기화와 String 초기화

보통 코드 작성시 String으로 대부분의 동작이 가능하지만, char로 바꾸어서 하나의 문자만 비교해야 하는 경우가 가끔 있다.

따라서 char 와 String의 초기화 방법을 잘 알아 두어야 한다.

```java
// String
String str = null; // (O) null 초기화 가능
String str = "";   // (O) 빈문자열 초기화 가능

// char
char c = null;     // (X) null 초기화 이렇게 하면 안됨
char c = '\u0000'; // (O) null 초기화는, unicode을 사용해야함, 다행이 null은 0000임, 싱글 쿼테이션 써야함
char c = '';       // (X) 빈문자열 초기화 불가능
char c = ' ';      // (O) 공백 하나 띄우고 초기화는 가능
```



