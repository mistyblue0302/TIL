## 인스턴스화를 막으려거든 private 생성자를 사용하라

추상 클래스를 만드는 것으로는 인스턴스화를 막을 수 없다. 
추상 클래스를 사용하여 클래스를 정의하면 해당 클래스를 직접 인스턴스화할 수 없지만, 하위 클래스를 만들어 인스턴스화할 수 있기 때문이다.

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
