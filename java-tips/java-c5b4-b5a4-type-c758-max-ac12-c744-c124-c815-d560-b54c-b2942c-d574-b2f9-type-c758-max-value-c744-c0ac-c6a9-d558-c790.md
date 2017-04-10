#### Java 어떤 Type의 Max 값을 설정할때는, 해당 Type의 MAX\_VALUE을 사용하자

Java에서, Integer나 Float, Double 의 Max 값을 설정해야 하는경우, 해당  Type에서 제공하는 MAX\_VALUE을 사용하면 간단하게 처리 가능하다.

```java
int int_INFINITY = Integer.MAX_VALUE;
float float_INFINITY = Float.MAX_VALUE;
double double_INFINITY = Double.MAX_VALUE;
```



