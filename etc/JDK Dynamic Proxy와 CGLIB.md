## JDK Dynamic Proxy와 CGLIB

### Proxy란

클라이언트 -> 프록시 -> 타겟

1. 클라이언트로부터 타겟을 대신해서 요청을 받는 대리인
2. 실제 오브젝트인 타겟은 프록시를 통해 최종적으로 요청받아 처리함.
3. 따라서 타겟은 자신의 기능에만 집중하고 부가기능은 프록시에게 위임함.

### Proxy 사용 목적

- 클라이언트가 타겟에 **접근하는 방법을 제어**하는 것
- 타겟에 **부가적인 기능을 부여**해주기 위한 것

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

Clien는 인터페이스를 가르키고, 인터페이스를 구현한 프록시 객체와 타겟 객체 두 가지를 생성해서 Client가 HelloProxy를 사용하도록 구현한다. HelloTarget 클래스를 변경하지 않고 부가적인 기능을 추가할 수 있게 되었다(개방 폐쇄 원칙). HelloTarget이 Hello를 붙여 반환하는 기존 코드가 해야 하는 일만 유지할 수 있다(단일 책임 원칙).

### Proxy 패턴 정리

**장점**

- OCP(개방 폐쇄 원칙) : 기존 코드를 변경하지 않고 새로운 기능을 추가 할 수 있다.
- SRP(단일 책임 원칙) : 기존 코드가 해야 하는 일만 유지할 수 있다.
- 기능 추가, 접근 제어 등 다양한 응용이 가능

**단점**

- 코드의 복잡도가 증가한다. 프록시를 구현하려면 인터페이스를 구현해야 하고 프록시 객체를 생성해주어야 되기 때문이다.
- 중복 코드가 발생한다. 위 예제의 toUpperCase는 모든 메소드에 적용되었는데, 이런 식으로 부가 기능을 추가할 경우 중복 코드가 발생한다.

> 어떻게 해결할까? Dynamic Proxy(동적 프록시)로 해결할 수 있다.

### JDK Dynamic Proxy란

**동작 방식**

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Dynamic%20Proxy1.png)

Dynamic Proxy는 프록시 팩토리에 의해 런타임 시 다이내믹하게 만들어지는 오브젝트다. Dynamic Proxy는 타깃의 인터페이스와 같은 타입으로 만들어진다.

1. 프록시 클래스를 직접 구현하지 않아도 됨 -> 코드 복잡도 해소
2. Invocation Handler -> 중복 코드 제거

위의 HelloProxy 클래스는 세 개의 메소드에 toUpperCase() 메소드가 중복되어 있다. 이러한 부분을 InvocationHandler를 통해 해결할 수 있다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Dynamic%20Proxy.png)

Dynamic Proxy로부터 요청을 받으려면 InvocationHandler를 구현해야 한다. sayHello(), sayHi(), sayThankYou() 메소드를 InvocationHandler 인터페이스에서 재정의한 invoke()를 통해 부가기능을 처리하여 타겟에게 다시 반환하는 형태로 해결할 수 있다. InvocationHandler 인터페이스의 특징은 타겟을 필드로 반드시 가지고 있어야 한다.

```java
import java.lang.reflect.Method; //Reflection API

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

InvocationHandler를 구현한 UpperCaseHandler는 메소드의 이름이 say로 시작하면 toUpperCase로 변환하여 반환하는 기능을 invoke() 메소드를 통해 재정의 하였다. 여기서 JDK Dynamic Proxy는 Method라는 Reflection API를 사용한다.

> Reflection API란?
>
> 구체적인 클래스 타입을 알지 못해도 **런타임**에 클래스의 정보에 접근할 수 있게 해주는 자바 API

Dynamic Proxy를 통해 요청이 전달되면 Reflection API를 이용해 타겟 오브젝트의 메소드를 호출한다. Reflection API는 동적일 때 해결되는 타입을 포함하므로 JVM의 optimization이 작동하지 않아 성능상 느리다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Dynamic%20Proxy2.png)

JDK Dynamic Proxy의 구현부는 위와 같다. 클라이언트가 메소드를 요청하면 JDK Dynamic Proxy는 메소드 처리를 InvocationHandler에게 위임한다. InvocationHandler는 부가 기능을 수행하고 다시 타겟에게 위임하는 형태로 진행된다.

```java
Hello hello = (Hello) Proxy.newProxyInstance(
      Main.class.getClassLoader(), //프록시 로딩에 사용할 클래스 로더
      new Class[]{Hello.class}, //타겟의 인터페이스
      new UpperCaseHandler(new HelloTarget()) //부가기능과 위임할 타겟
);
```

Proxy.newProxyInstance를 사용해서 JDK Dynamic Proxy를 사용할 수 있는데, 처음에는 프록시 로딩에 사용할 클래스 로더와, 타겟의 인터페이스를 적어준 후 부가기능과 위임할 타겟을 정한다.

### JDK Dynamic Proxy의 특징

1. JDK에서 지원하는 프록시 생성 방법으로 외부 라이브러리에 의존하지 않는다.
2. Reflection API를 사용하기 때문에 느리다.
3. 인터페이스가 반드시 있어야 한다.
4. Invocation Handler를 재정의한 invoke를 구현해줘야 부가기능이 추가된다.

### CGLIB란

CGLIB는 Code Generator Libray의 약자로, 클래스의 바이트 코드를 조작하여 프록시 객체를 생성해 주는 라이브러리이며 동적 프록시 중 하나이다. 스프링에서는 클라이언트가 메소드를 요청하면 ProxyFactoryBean에서 인터페이스 유무를 확인하고 인터페이스가 있으면 JDK Dynamic Proxy를 호출하고, 없으면 CGLIB 방식으로 프록시를 생성한다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/CGLIB2.png)

- 상속을 통한 프록시 구현
- 바이트 코드를 조작해서 프록시 생성
- MethodInterceptor를 재정의한 intercept() 메소드를 재정의해야 부가기능이 추가된다.

![img4](https://github.com/dilmah0203/TIL/blob/main/Image/CGLIB.png)

클라이언트가 메소드를 요청하면 CGLIB에서는 InvocationHandler가 아닌 MethodInterceptor에 메소드 처리를 요청한다. 그럼 MethodInterceptor에서 부가 기능을 수행 후 타겟에 위임하게 된다. Enhancer라는 외부 의존성을 사용하여 CGLIB를 사용할 수 있다.

### CGLIB의 특징

1. 인터페이스에도 강제로 적용할 수 있다. 이때는 클래스에도 프록시를 적용시켜야 한다.
2. 상속을 이용해 프록시를 만들기 때문에 메소드에 final을 붙이면 안된다.
3. Enhancer 의존성을 추가해야 한다.
4. Default 생성자 필요
5. 타겟의 생성자 두 번 호출

### JDK Dynamic Proxy vs CGLIB

CGLIB는 메소드가 처음 호출 되었을 때 동적으로 타깃의 클래스의 바이트 코드를 조작하고, 이후 호출시엔 조작된 바이트 코드를 재사용하기 때문에 성능 면에서 빠르다. JDK Dynamic Proxy는 Reflection을 사용해 느리다.

### Spring의 프록시 구현

Spring은 CGLIB이 기본으로 작동한다. Spring에서 지원하는 프록시 생성 방법으로, ProxyFactoryBean을 제공한다. ProxyFactoryBean은 CGLIB를 만들기 위해 타깃의 인터페이스 정보가 필요없다. JDK Dynamic Proxy는 Invocation Handler를 통해 프록시 부가기능을 구현했지만, ProxyFactoryBean은 MethodInterceptor 인터페이스를 재정의한 invoke() 메소드를 구현해줘야 부가 기능이 추가된다.

![img5](https://github.com/dilmah0203/TIL/blob/main/Image/ProxyFactoryBean.png)

InvocationHandler의 특징은 타깃을 필드로 반드시 가지고 있어야 하기 때문에 타깃에 의존적이다. ProxyFactoryBean은 MethodInterceptor을 통해 이러한 단점을 극복했다. ProxyFactoryBean이 프록시를 생성하면 부가기능을 MethodInterceptor가 처리해준다. 그런데 이 때 타깃, 즉 실제 원하는 기능을 가지고 있는 객체는 프록시가 가지고 있는다.

> 타깃을 왜 가지지 않을까?

MethodInterceptor는 타깃을 가지지 않는데, 이유는 부가 기능을 독립적으로 유지하기 위해서이다. 즉 부가기능을 **싱글톤으로 공유**하여 사용 가능하다. 그렇기 때문에 ProxyFactoryBean은 MethodInterceptor를 사용한다. 하지만 ProxyFactoryBean도 개발자가 코드로 매번 생성하는 코드를 주입해줘야하는 단점이 있다.

<br>

참고

[우아한Tech JDK Dynamic Proxy와 CGLIB](https://www.youtube.com/watch?v=MFckVKrJLRQ)

[https://docs.oracle.com/javase/tutorial/reflect/index.html](https://docs.oracle.com/javase/tutorial/reflect/index.html)
