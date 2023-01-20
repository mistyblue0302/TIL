## Nested Classes

Java에선 class 안의 또 다른 class를 허용하는데 이러한 class를 nested class(중첩 클래스)라고 한다. nested class는 static과 non-static으로 나뉜다. static으로 선언되어 있는 nested class는 static nested class이며 non-static nested class는 inner class이다.

```java
class OuterClass {
    class InnerClass {
    }
    
    static class StaticNestedClass {
    }
}
```

왜 사용할까?

1. 한 곳에서만 사용되는 class를 논리적으로 그룹화한다. 만약 어떤 class가 다른 하나의 class에서만 유용하다면 해당 class에 포함시킴으로써 용도가 보다 명확해진다.

2. 캡슐화가 필요할 때 사용한다. 같은 패키지 내의 다른 클래스들로부터 nested class를 숨길 수 있다.

## Static nested class

Static nested class의 경우 다음과 같이 객체를 생성할 수 있다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Static%20nested%20class.png)

static으로 선언된 내부 class 안에서는 static으로 선언된 변수와 메소드만 사용 가능하다. 하지만 외부 class의 멤버에 접근할 시 객체를 생성해야 한다.

```java
public class OuterClass {
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
        OuterClass.StaticNested outerObject = new OuterClass.StaticNested();
        outerObject.get();
    }
}
```

## Non-static nested class(Inner class)

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Inner%20class.png)

먼저 외부 class의 객체를 생성한 후 내부 class의 객체를 생성해야 한다. static nested class와는 달리 해당 객체의 메소드와 필드에 직접 접근할 수 있다. 내부 class를 인스턴스화하려면 먼저 외부 class를 인스턴스화해야 한다.

```java
public class OuterClass {
    static int x = 3;
    int y = 2;

    public int z = 10;

    class InnerClass {
        void get() {
            System.out.println("x: " + x); //3
            System.out.println("y: " + y); //2
            System.out.println("z: " + z); //10
        }
    }
}

public class Main {
    public static void main(String[] args) {
        OuterClass outerObject = new OuterClass();
        OuterClass.InnerClass innerObject = outerObject.new InnerClass();
        innerObject.get();
    }
}
```

<br>

참고

https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html

https://www.geeksforgeeks.org/difference-between-static-and-non-static-nested-class-in-java/

