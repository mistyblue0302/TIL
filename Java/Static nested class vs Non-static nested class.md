## Nested Classes

nested class(중첩 클래스)란 class안에 선언된 또 다른 class를 의미한다. nested class는 static과 non-static으로 나뉜다. 

```java
class OuterClass {
    class InnerClass {
    }
    
    static class StaticNestedClass {
    }
}
```

왜 사용할까?

1. 한 곳에서만 사용되는 class를 **논리적으로 그룹화**한다. 만약 어떤 class가 다른 하나의 class와만 관계가 있다면, 외부 class를 새로 작성하는 것이 아닌 내부 class로 작성함으로써 용도가 보다 명확해진다.

2. B class가 A class의 멤버에 대해 접근이 필요할 때 B class를 A class의 nested class로 만들어 접근을 가능하게 하면서도 A class의 멤버들은 private으로 선언이 가능해지면서 캡슐화할 수 있다.

## Static nested class

- static이 붙는 nested class
- 외부 class의 다른 멤버를 직접 참조가 불가능(static 멤버는 사용가능)
- static의 특징에 따라 **외부 클래스의 인스턴스 생성 없이** 내부 class를 접근하기 위한 용도

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Static%20nested%20class.png)

```java
class OuterClass {

    static int x = 10;
    int y = 20;
    private static int z = 30;

    static class StaticNestedClass {
        void get() {
            System.out.println("x: " + x); //x:10
            //System.out.println("y: " + y);
            System.out.println("z: " + z); //z:30
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        OuterClass.StaticNestedClass staticIneer = new OuterClass.StaticNestedClass();
        staticIneer.get();
   }
}
```

외부 class의 private 멤버에도 접근할 수 있다는 점을 제외하고는 일반 class와 쓰임새는 동일하다.

## Non-static nested class(Inner class)

외부 class의 객체를 생성한 뒤 내부 class의 객체를 생성해야 한다. 즉, **외부 class에 대한 참조가 필요하다.**

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Inner%20class.png)

-  외부 class의 인스턴스에 대한 참조가 유지된다.
-  내부 class를 new를 통해 인스턴스를 할당해줌으로써 멤버변수처럼 사용 가능
-  외부 class에 대한 참조가 유지되므로 내부 class도 외부 class의 필드를 사용 할 수 있다.

```java
class OuterClass {

    static int x = 10;
    int y = 20;
    private static int z = 30;

    class InnerClass {
        void get() {
            System.out.println("x: " + x); //x:10
            System.out.println("y: " + y); //y:20
            System.out.println("z: " + z); //z"30
        }
    }
}
```

```java
public class Main {
   public static void main(String[] args) {
        OuterClass outerObject = new OuterClass();
        OuterClass.InnerClass innerObject = outerObject.new InnerClass();
        innerObject.get();
   }
}
```

내부 class는 외부 class의 멤버이므로 접근 제어자(private, public, protected, default)를 사용할 수 있다. Non-static nested class의 인스턴스는 외부 class의 인스턴스와 연결되기 때문에, 외부 class가 사용되지 않더라도 내부 class의 참조로 인해 GC가 수거하지 못하여 메모리 해제를 하지 못하는 경우가 발생할 수 있다.

<br>

참고

https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html

https://www.geeksforgeeks.org/difference-between-static-and-non-static-nested-class-in-java/

