## IoC(Inversion of Control)란?

스프링 애플리케이션에서는 오브젝트(빈)의 생성과 의존 관계 설정, 사용, 제거 등의 작업을 애플리케이션 코드 대신 스프링 컨테이너가 담당한다. 이를 스프링 컨테이너가 오브젝트에 대한 제어권을 가진다고 하여 IoC라고 부른다. 따라서 스프링 컨테이너를 IoC 컨테이너라고도 부른다.

## DIP(Dependency Inversion Principle)란?

고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 되며, 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다는 것이다. 여기서 각각 고수준 모듈과 저수준 모듈이란 다음을 의미한다.

- 고수준 모듈 : 변경이 없는 추상화된 클래스(또는 인터페이스)
- 저수준 모듈 : 변하기 쉬운 구체 클래스

의존 역전 원칙(DIP)이란 결국 **추상화에 의존하며 구체화에는 의존하지 않는 설계 원칙**을 의미한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/DIP.png)

SimplePasswordEncoder는 변하기 쉬운 구체 클래스인데, UserService가 SimplePasswordEncoder에 직접 의존하는 것은 의존성 역전 원칙에 위배된다. 그렇기 때문에 UserService는 변하지 않는 추상화에 의존하도록 변경해야 하며 PasswordEncoder 인터페이스를 통해 이에 의존하도록 해야한다. UserService가 추상화된 PasswordEncoder에 의존하므로 비밀번호에 대한 암호화 정책이 변경되어도 다른 곳들로 변경이 전파되지 않는다.

## DI(Dependency Injection)란?

의존성이란 한 클래스가 변경될 때 다른 클래스가 영향을 받는 것을 말하며 의존성 주입(DI)이란 외부에서 의존성을 주입해주는 것을 말한다.

> 의존성 주입은 어떻게 할까?

### 1. 필드 주입

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private MemberService memberService;

}
```

- `@Autowired` 애노테이션을 사용해 의존성 주입을 한다.
- 애플리케이션 컨텍스트는 일치하는 의존성을 검색한다.
  - 이 때 일치하는 항목이 1개라면 : 찾고 있는 의존성을 주입
  - 일치하는 항목이 2개 이상일 경우 @Primary나 @Qualifier를 사용할 수 있다.

### 2. Setter 주입

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

- 필드 값을 변경하는 Setter를 통해서 의존 관계를 주입하는 방법
- Setter 주입은 생성자 주입과 다르게 주입받는 객체가 변경될 가능성이 있는 경우나 의존성을 선택적으로 주입할 때 사용한다.

### 3. 생성자 주입

```java
@Service
public class UserService {

  private final UserRepository userRepository;
  private final MemberService memberService;

  public UserService (UserRepository userRepository, MemberService memberService) {
        this.userRepository = userRepository;
        this.memberService = memberService;
    }
}
```

- 생성자 주입은 객체의 최초 생성 시점에 1회 호출되기 때문에 **불변**하게 사용할 수 있다.
- **final** 키워드를 붙여 의존성의 누락을 막을 수 있다.
- Lombok과 결합되어 코드를 간결하게 작성할 수 있다. Lombok에는 final 변수를 위한 생성자를 대신 생성해주는 @RequiredArgsConstructor를 쓸 수 있다.

<br>

참고

https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction(https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction)


