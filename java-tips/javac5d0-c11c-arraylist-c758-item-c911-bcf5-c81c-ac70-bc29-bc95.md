#### Java에서 ArrayList의 item 중복 제거 방법

java에서 ArrayList에 generic type을 저장 할 수 있다. 있다. 중복된 item을 제거하고 싶은 경우가 발생 할 수 있다.

java에서 ArrayList \(혹은 List Type\)의 item \(사용자가 정의한 클래스의 인스턴스 item들\)의 중복 item을 제거하는 방법을 소개한다.

java에서 ArrayList의 item 중복을 초기화 하는 방법은 매우 간단하다. HashSet에 Collection\(List\)을 넣은뒤 다시 해당 HashSet을 List로 new하면 된다. 아래 코드를 보자

아래 코드는 사용자 정의 class인 LiRi을 담는 ArrayLIst에서, 중복된 LiRi을 제거하는 코드이다.

```java

// 3개의 동일 item을 넣음
ArrayList<LiRi> list = new ArrayList<LiRi>();
list.add(new LiRi(0, 1));
list.add(new LiRi(0, 1));
list.add(new LiRi(0, 1));


// 우리가 원하는건 중복이 제거된 LiRi(0, 1) 한개의 item 만 남기를 원함
// 아래와 같이 HashSet에 넣고 이를 다시 ArrayList로 new하면 중복이 제거된 List을 얻을 수 있음
ArrayList<LiRi> newlist = new ArrayList<LiRi>(new HashSet<LiRi>(list));


// 주의 사항, HashSet을 이용해서 중복을 제거하려면, hashCode 함수와, equals 함수가 있어야 한다.
// 따라서 개발자가 정의하는 class의 경우 해당 함수를 구현해 주어야 한다.
// String type의 경우 hashCode() 함수가 있다. 이를 사용해서, 본인이 정의한 class의 class변수값들을
// 적절히 사용해서, unique한 hash code값이 나오도록 만들면 된다.
class LiRi{
    int Li, Ri;

    public LiRi(int li, int ri) {
        super();
        Li = li;
        Ri = ri;
    }

    @Override
    public int hashCode() {
        return String.valueOf(this.Li).hashCode() + String.valueOf(this.Ri).hashCode();
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof LiRi) {
            LiRi temp = (LiRi) obj;
            return temp.Li == this.Li && temp.Ri == this.Ri;
        }
        return false;
    }
}



```



