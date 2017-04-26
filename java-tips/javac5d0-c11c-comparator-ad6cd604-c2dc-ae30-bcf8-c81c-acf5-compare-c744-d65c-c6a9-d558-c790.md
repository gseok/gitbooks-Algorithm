#### Java에서 Comparator 구현시 기본 제공 compare을 활용하자

Java에서 본인이 생성한 클래스나 자료구조를 정렬하기 위해서 Comparator\(Comparable 역시 동일하다\)을 구현해야 하는 경우가 있다. 이때 개발자가 직접 값을 비교해서 구현을 하는 것도 가능하지만. 만약 비교해야 하는 값이 Java에서 기본 제공되고 있는 Type 형태라면, 별도의 구현 없이 해당 Type의 Comparator을 바로 사용하면 간단하게 구현을 할 수 있다.

```java
class MyTestClass {
    int dist;

    public MyTestClass(int dist) {
        super();
        this.dist = dist;
    }
}

// 위와 같이 자신이 정의한 클래스가 있다고 가정한다.
MyTestClass classList[] = new MyTestClass[10];

// 배열에 10개의 클래스를 저장했다고 가정한다.
for (int i = 0; i < 10; i++) {
    classList[i] = new MyTestClass(i + 100);
}

// 해당 배열을 정렬하기 위해서, Comparator 을 구현한다.
Arrays.sort(classList, new Comparator<MyTestClass>() {
    @Override
    public int compare(MyTestClass o1, MyTestClass o2) {
        // dist는 int(Integer) Type임
        // 따라서 Integer.compare을 바로 사용하면 됨.
        return Integer.compare(o1.dist, o2.dist);
        
        // 만약 정렬을 역순으로 하고 싶으면 아래와 같이 순서를 바꾸면됨.
        // return Integer.compare(o2.dist, o1.dist);
    }
});
```

위 예제와 같이 Comparator 구현시, 비교해야 하는 값이 이미 Java에서 제공하고 있는 Type이면, 간단하게 해당 Type의 compare함수를 사용하면 된다.

위에서는 Comparator를 소개하였는데 Comparable을 상속하여, CompareTo을 구현해야 하는 경우 역시 동일한 방법으로 가능하다.

```java
class MyTestClass implements Comparable<MyTestClass> {
    int dist;

    public MyTestClass(int dist) {
        super();
        this.dist = dist;
    }

    public int compareTo(MyTestClass o) {
        int a = this.dist;
        int b = o.dist;
        return Integer.valueOf(a).compareTo(Integer.valueOf(b));
     
        // 반대반향을 원하면, a,b를 바꾸어서.
        // return Integer.valueOf(b).compareTo(Integer.valueOf(a));
    }
}

```

사실 0을 기준으로, 0보다 작으면 더 작음, 0이면 동일, 0보다 크면 큼. 으로 간단하지만, 본인이 a &gt; b ? 1 : -1; 와 같은 형식으로 구현하지 않아도 기존 Type으로 간단히 구현 할 수 있고, 정렬의 반대는 간단하게 param의 순서만 바꾸어서 구현 가능하다.

또한 class에서 복잡한 형태의 비교를 통해서 compare나 compareTo를 작성해야 하는 경우에도 기본 Type의 compare나 compareTo을 활용하면 좀 더 쉽게 구현이 가능하다.



