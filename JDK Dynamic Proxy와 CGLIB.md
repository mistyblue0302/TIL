## JDK Dynamic Proxy와 CGLIB

## Proxy란?

클라이언트 -> 프록시 -> 타겟

1. 클라이언트로부터 타겟을 대신해서 요청을 받는 대리인
2. 실제 오브젝트인 타겟은 프록시를 통해 최종적으로 요청받아 처리함.
3. 따라서 타겟은 자신의 기능에만 집중하고 부가기능은 프록시에게 위임함.

## Proxy 사용 목적

- 클라이언트가 타깃에 **접근하는 방법을 제어**하는 것
- 타깃에 **부가적인 기능을 부여**해주기 위한 것

프록시 패턴을 통해 Proxy를 직접 구현할 수 있다.

```java
public class Client {

  public static void main(String[] args) {
    Hello hello = new Hello();
    System.out.println(hello.sayHello("ju seon")); //Hello ju seon
  }
}
```

```java
public class Hello {

  public String sayHello(String name) {
    return "Hello " + name;
  }

  public String sayHi(String name) {
    return "Hi " + name;
  }

  public String sayThankYou(String name) {
    return "Thank You " + name;
  }
}
```

Client와 Hello 클래스 수정 없이 출력문 "Hello ju seon"을 대문자로 반환(부가 기능)하고 싶을 경우, 프록시 패턴을 사용할 수 있다. Hello 클래스를 인터페이스로 변경하여 선언부를 생성하고, Hello 인터페이스를 구현한 HelloTarget 클래스를 생성한다.

```java
public interface Hello {

  String sayHello(String name);
  String sayHi(String name);
  String sayThankYou(String name);
}
```

```java
public class HelloTarget implements Hello {

  @Override
  public String sayHello(String name){
    return "Hello " + name;
  }

  @Override
  public String sayHi(String name) {
    return "Hi " + name;
  }

  @Override
  public String sayThankYou(String name) {
    return "Thank You " + name;
  }
}
```

다음으로 프록시 객체를 생성하고 이 때 대문자로 변환하기 위해 toUpperCase()를 호출한다. 프록시는 하나의 특징을 가지는데, 필드로 인터페이스 선언부를 가지고 있다. 왜냐하면 특정 시점에서 타겟 객체의 메소드를 호출 해주기 위함이다.

```java
public class HelloProxy implements Hello {

  private Hello hello;

  public HelloProxy(Hello hello) {
    this.hello = hello;
  }

  @Override
  public String sayHello(String name){
    return hello.sayHello(name).toUpperCase();
  }

  @Override
  public String sayHi(String name){
    return hello.sayHi(name).toUpperCase();
  }

  @Override
  public String sayThankYou(String name){
    return hello.sayThankYou(name).toUpperCase();
  }
}
```

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Proxy%20pattern.png)

Client 쪽에서 인터페이스를 가르키고, 인터페이스를 구현한 프록시 객체와 타겟 객체 두 가지를 생성해서 Client가 HelloProxy를 사용하도록 구현한다. HelloTarget 클래스를 변경하지 않고 부가적인 기능을 추가할 수 있게 되었다. HelloTarget이 Hello를 붙여 반환하는 기존 코드가 해야 하는 일만 유지할 수 있다.

## Proxy 패턴 정리

**장점**

- OCP(개방-폐쇄 원칙) : 기존 코드를 변경하지 않고 새로운 기능을 추가 할 수 있다.
- SRP(단일 책임 원칙) : 기존 코드가 해야 하는 일만 유지할 수 있다.

**단점**

- 코드의 복잡도가 증가한다. 프록시를 구현하려면 인터페이스를 구현해야 하고 프록시 객체를 생성해주어야 되기 때문이다.
- 중복 코드가 발생한다. 위 예제의 toUpperCase는 모든 메소드에 적용되었는데, 이런 식으로 부가 기능을 추가할 경우 중복 코드가 발생한다.

> 어떻게 해결할까? Dynamic Proxy로 해결할 수 있다.

## Dynamic Proxy

**동작 방식**

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Dynamic%20Proxy1.png)

Dynamic Proxy는 프록시 팩토리에 의해 런타임 시 다이내믹하게 만들어지는 오브젝트다. Dynamic Proxy는 타깃의 인터페이스와 같은 타입으로 만들어진다.

1. 프록시 클래스를 직접 구현하지 않아도 됨 -> 코드 복잡도 해소
2. Invocation Handler -> 중복 코드 제거

위의 HelloProxy 클래스는 세 개의 메소드에 toUpperCase() 메소드가 중복되어 있다. 이러한 부분을 InvocationHandler를 통해 해결할 수 있다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Dynamic%20Proxy.png)

Dynamic Proxy로부터 요청을 받으려면 InvocationHandler를 구현해야 한다. sayHello(), sayHi(), sayThankYou() 메소드를 InvocationHandler 인터페이스에서 재정의한 invoke를 통해 부가기능을 추가하여 타겟에게 다시 반환하는 형태로 해결할 수 있다.

```java
import java.lang.reflect.Method;

public class UpperCaseHandler implements InvocationHandler {
  private final Object target;

  public UpperCaseHandler(Object target) {
    this.target = target;
  }

  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    if (method.getName().startsWith("say")) { //메소드의 이름이 say로 시작하면 toUpperCase로 변환하여 반환
      return ((String) method.invoke(target, args)).toUpperCase();
    }
    return method.invoke(target, args);
  }
}
```

Dynamic Proxy는 Method라는 Reflection API를 사용한다.

> Reflection API란? 구체적인 클래스 타입을 알지 못해도 **런타임**에 클래스의 정보에 접근할 수 있게 해주는 자바 API

Dynamic Proxy를 통해 요청이 전달되면 Reflection API를 이용해 타겟 오브젝트의 메소드를 호출한다.

```java
Hello hello = (Hello) Proxy.newProxyInstance(
      Main.class.getClassLoader(), //프록시 로딩에 사용할 클래스 로더
      new Class[]{Hello.class}, //타겟의 인터페이스
      new UpperCaseHandler(new HelloTarget()) //부가기능과 위임할 타겟
);
```
