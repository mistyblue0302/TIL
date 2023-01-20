## Static nested class vs Non-static nested class

Java에선 class 안의 또 다른 class를 허용하는데 이러한 class를 nested class(중첩 클래스)라고 한다. nested class는 static과 non-static으로 나뉜다. Non-static nested class를 inner class라고 하며, static으로 선언되어 있는 nested class는 static nested class이다.

```java
class OuterClass {
    class InnerClass {
    }
    
    static class StaticNestedClass {
    }
}
```

<br>

**static nested class**

static으로 선언된 내부 class 안에서는 static으로 선언된 변수와 메소드만 사용 가능하다. 하지만 외부 class의 멤버에 접근할 시 객체를 생성해야 한다.

```java
public class Ex {

    static int x = 3;
    int y = 2;

    private static int z = 10;

    static class StaticNested {

        void get() {
            System.out.println("x: " + x); //3
            System.out.println("z: " + z); //10
        }
    }
}

public class Main {

    public static void main(String[] args) {
        Ex.StaticNested e = new Ex.StaticNested();
        e.get();
    }

}
```

**왜 쓸까?**

한 곳에서만 사용되는 class를 논리적으로 그룹화한다. 만약 어떤 class가 다른 하나의 class에서만 유용하다면 해당 class에 포함시킴으로써 용도가 보다 명확해진다.

<br>

**non-static nested class**

먼저 외부 class의 객체를 생성한 후 내부 class의 객체를 생성해야 한다. static nested class와는 달리 어떤 변수라도 참조할 수 있다.

```java
public class Ex {

    static int x = 3;
    int y = 2;

    public int z = 10;

    class NonNested {

        void get() {
            System.out.println("x: " + x); //3
            System.out.println("y: " + y); //2
            System.out.println("z: " + z); //10
        }
    }
}

public class Main {

    public static void main(String[] args) {
        Ex e1 = new Ex();
        Ex.NonNested e2 = e1.new NonNested();
        e2.get();
    }

}

```

**왜 쓸까?**

캡슐화가 필요할 때 사용한다. 만약 A라는 class에 private 변수가 있고, 이 변수에 접근하고 싶은 B라는 class가 있다고 가정하자. B class를 외부에 노출하고 싶지 않을 경우에 사용할 수 있다.

<br>

**gc**

non-static nested class는 외부 class 멤버에 접근 할 수 있다고 하였다. 즉, 실행동안 외부 class의 모든 객체는 내부 class와 함께 메모리에 있어야 한다. gc는 인스턴스를 수거 하지 못하여 메모리 누수가 생길 수 있다.
 
<br>

참고

https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html

https://www.geeksforgeeks.org/difference-between-static-and-non-static-nested-class-in-java/

