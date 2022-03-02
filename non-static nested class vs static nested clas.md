# 💡 **non-static nested class vs static nested class**

- non-static nested class

  - Inner class라고 하며 외부 인스턴스에 대한 참조가 유지된다

  - 외부 인스턴스는 내부 클래스를 new를 통한 인스턴스 할당으로 멤버변수처럼 사용 가능하다

  - 내부 클래스도 외부 클래스의 자원을 사용할 수 있다

<br>

```java
public class Outer{
    class Inner{

    }
}

Outer outer = new Outer();
Outer.Inner inner = new Outer.new Inner();
```

- static nested class

  - static이 붙는 중첩 클래스

  - static 멤버 사용가능

  - 외부 인스턴스 멤버의 직접참조가 불가능

<br>

```java
public class Outer{
    static class staticNested{

    }
}

Outer.Inner staticNested = new Outer.Inner();
```

외부 참조가 유지된다는 것은 메모리에 대한 참조가 유지된다는 뜻 <br>
GC가 메모리를 회수할 수 없다. 성능상 비효율적
