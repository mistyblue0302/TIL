## 인스턴스화를 막으려거든 private 생성자를 사용하라

추상 클래스를 만드는 것으로는 인스턴스화를 막을 수 없다. 
추상 클래스를 사용하여 클래스를 정의하면 해당 클래스를 직접 인스턴스화할 수 없지만, 하위 클래스를 만들어 인스턴스화할 수 있기 때문이다.

그리고 아무런 생성자를 만들지 않은 경우 컴파일러가 기본적으로 아무 인자가 없는 public 생성자를 만들어 주기 때문에 그런 경우에도 인스턴스를 생성할 수 있다.

```java
public abstract class UtilityClass {

    public abstract void doSomething();

    public static UtilityClass createInstance() {
        return new AnotherClass();
    }
}
```

```java
public class AnotherClass extends UtilityClass {

    @Override
    public void doSomething() {
        System.out.println("do something");
    }
}
```

```java
//UtilityClass utilityClass = new UtilityClass(); //컴파일 오류(인스턴스화할 수 없다)

//하위 클래스를 사용하여 인스턴스 생성
UtilityClass utility = UtilityClass.createInstance();
utility.doSomething();
```

명시적으로 private 생성자를 추가하여 클래스의 인스턴스화를 막을 수 있다.

```java
public class UtilityClass {

    //인스턴스를 만들지 못하게 막는다.
    private UtilityClass() {
        throw new AssertionError();
    }

    public static String getName() {
        return "juseon";
    }
}
```

```java
//UtilityClass utilityClass = new UtilityClass(); //컴파일 오류(인스턴스화할 수 없다)
UtilityClass.getName();
```

부가적으로 상속도 막을 수 있다. 상속의 경우에 상위 클래스의 생성자를 호출해야 하는데, 이 클래스의 생성자가 private이라 호출이 막혔기 때문에 상속을 할 수 없다.
