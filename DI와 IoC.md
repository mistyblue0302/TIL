## DI와 IoC

### IoC(Inversion of Control)란?

```java
public class A {
  private B b;

  public A() {
    this.b = new B();
  }
}
```

A 클래스는 B를 필드로 가지고 있고 생성자 내부에서 필드를 초기화하고 있다. 

```java
public class A {
  private B b;

  public A(B b) {
    this.b = b;
  }
}
```
위 코드에서는 외부로부터 B를 받아 초기화하고 있다. 이렇게 의존 객체를 직접 생성하여 사용하지 않고, 주입받아 사용하는 것을 제어의 역전(IoC)이라고 한다.

> 왜 필요할까?

객체 내부에서 생성자를 통해 다른 객체를 초기화하는 것은 결합도가 높은 코드라고 할 수 있다. 하지만 이처럼 외부에서 의존성을 주입받음으로써 역할과 책임을 분리해 객체의 응집도를 높이고 이에 따라 변경에 유연한 코드를 작성할 수 있다.

### DIP(Dependency Inversion Principle)란?

고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 되며, 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다는 것이다. 여기서 각각 고수준 모듈과 저수준 모듈이란 다음을 의미한다.

- 고수준 모듈 : 변경이 없는 추상화된 클래스(또는 인터페이스)
- 저수준 모듈 : 변하기 쉬운 구체 클래스

의존 역전 원칙(DIP)이란 결국 **추상화에 의존하며 구체화에는 의존하지 않는 설계 원칙**을 의미한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/DIP.png)

SimplePasswordEncoder는 변하기 쉬운 구체 클래스인데, UserService가 SimplePasswordEncoder에 직접 의존하는 것은 의존성 역전 원칙에 위배된다. 그렇기 때문에 UserService는 변하지 않는 추상화에 의존하도록 변경해야 하며 PasswordEncoder 인터페이스를 통해 이에 의존하도록 해야한다. UserService가 추상화된 PasswordEncoder에 의존하므로 비밀번호에 대한 암호화 정책이 변경되어도 다른 곳들로 변경이 전파되지 않는다.

### DI(Dependency Injection)란?

의존성이란 한 클래스가 변경될 때 다른 클래스가 영향을 받는 것을 말하며 의존성 주입(DI)이란 외부에서 의존성을 주입해주는 것을 말한다.

> 의존성 주입은 어떻게 할까?

1. **생성자 주입**

```java
@Service
public class UserService {

  private UserRepository userRepository;
  private MemberService memberService;

  public UserService (UserRepository userRepository, MemberService memberService) {
        this.userRepository = userRepository;
        this.memberService = memberService;
    }
}
```

생성자 주입은 객체의 최초 생성 시점에 1회 호출 되는 것이 보장되기 때문에 주입받은 객체가 변하지 않거나, 반드시 객체의 주입이 필요한 경우에 강제하기 위해 사용할 수 있다. 

2. **Setter 주입**

```java
@Service
public class UserService {

    private UserRepository userRepository;
    private MemberService memberService;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Autowired
    public void setMemberService(MemberService memberService) {
        this.memberService = memberService;
    }
}
```
필드 값을 변경하는 Setter를 통해서 의존 관계를 주입하는 방법이다. Setter 주입은 생성자 주입과 다르게 주입받는 객체가 변경될 가능성이 있는 경우나 의존성을 선택적으로 주입할 때 사용한다.

3. **필드 주입**

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;
    @Autowired
    private MemberService memberService;

}
```

필드 주입은 외부에서 객체 수정이 불가능 하므로 테스트 코드 작 성시 어려움이 있다.

<br>

참고

[우아한Tech DI와 IoC](https://www.youtube.com/watch?v=8lp_nHicYd4)

