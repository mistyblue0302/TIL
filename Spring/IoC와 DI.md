## 1장. IoC 컨테이너와 DI

### 1. 제어의 역전(IoC : Inversion of Control)

- 스프링 애플리케이션에서는 오브젝트(빈)의 생성과 의존 관계 설정, 사용, 제거 등의 작업을 애플리케이션 코드 대신 스프링 컨테이너가 담당한다. 
- 이를 스프링 컨테이너가 오브젝트에 대한 제어권을 가진다고 하여 IoC라고 부른다. 따라서 스프링 컨테이너를 IoC 컨테이너라고도 부른다.

스프링에서는 IoC를 담당하는 컨테이너를 **bean Factory** 또는 **Application Context**이라고 부른다. 빈 팩토리라고 말할 때는 빈을 생성하고 관계를 설정하는 IoC의 기본 기능에 초점을 맞춘 것이고, 애플리케이션 컨텍스트라고 말할 때는 애플레케이션 전반에 걸쳐 모든 구성요소의 제어 작업을 담당하는 IoC 엔진이라는 의미가 있다.

제어의 역전이 사용되는 사례는 여러가지가 있다.

먼저 **서블릿**은 서블릿에 대한 제어권을 가진 컨테이너가 적절한 시점에 서블릿 클래스의 오브젝트를 생성하고 그 안의 메소드를 호출한다. 그리고 **디자인 패턴**인 템플릿 메소드 패에서도 찾을 수 있다. 제어권을 상위 템플릿 메소드에 넘기고, 자신은 필요할 때 호출되어 사용되도록 한다는 제어의 역전 개념이 들어있다. 

**프레임워크**도 제어의 역전 개념이 적용된 대표적인 기술이다. 라이브러리를 사용하는 애플리케이션 코드는 애플리케이션 흐름을 직접 제어한다. 단지 동작하는 중에 필요한 기능이 있을 때 능동적으로 라이브러리를 사용할 뿐이다. 반면에 프레임워크는 거꾸로 애플리케이션 코드가 프레임워크에 의해 사용된다. 보통 프레임워크 위에 개발한 클래스를 등록해두고, 프레임워크가 흐름을 주도하는 중에 개발자가 만든 애플리케이션 코드를 사용하도록 만드는 방식이다.

스프링 IoC 컨테이너는 각각의 **빈에 대한 정보를 담은 메타정보를 읽어들인 뒤에 이를 참고하여 빈 오브젝트를 생성하고 프로퍼티나 생성자를 통해 의존 오브젝트를 주입해주는 DI 작업을 수행**한다.

### 2. IoC 컨테이너의 종류
 
**BeanFactory**

 BeanFactory 인터페이스를 구현한 클래스는 단순히 컨테이너에서 객체를 생성하고 DI를 처리하는 기능만 제공한다. Bean을 등록, 생성, 조회, 반환 관리를 한다. Bean을 조회할 수 있는 getBean() 메소드가 정의되어 있다.
 
 
**ApplicationContext**

Bean을 등록, 생성, 조회, 반환 관리하는 기능은 BeanFactory와 같다. 메시지, 프로필, 환경 변수 등을 처리할 수 있는 기능을 추가로 정의한다.

- GenericApplicationContext : 일반적인 애플리케이션 컨택스트이다. 외부 파일(XML 등)로부터 빈 설정 메타정보를 전달받아 사용한다.

```java
GenericApplicationContext context = new GenericApplicationContext();

XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(context);
reader.loadBeanDefinitions("외부 파일 위치");
context.refresh(); //빈에 대한 메타정보를 읽어들였으니 application context를화초기화

Hello hello = context.getBean(Hello.class);
hello.hi();
```

XML 설정정보 리더인 XmlBeanDefinitionReader를 통해 GenericApplicationContext 이용하도록 하여 Hello bean을 사용하게 만든 코드이다. JUnit 테스트에서 생성되는 Application Context가 GenericApplicationContext이다.

- WebApplicationContext :  스프링에서 가장 많이 활용하는애플리케이션 컨텍스트로 웹 환경에서 사용할 때 필요한 기능이 추가된 어플리케이션 컨텍스트이다. 

다음 그림은 웹 환경에서 스프링 빈으로 이뤄진 애플리케이션이 동작하는 구조다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/WebApplicationContext.png)

서블릿 컨테이너가 브라우저로부터 오는 HTTP 요청을 받아 해당 요청에 매핑되어 있는 서블릿을 실행해주는 방식으로 동작한다.

### 3. IoC 컨테이너 계층구조

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/ApplicationContext.png)

각자 독립적으로 자신이 관리하는 빈을 갖고 있지만 DI를 위해 빈을 찾을 때는 부모 애플리케이션 컨텍스트의 빈까지 모두 검색한다. 만약, 형제 Context에 찾고자 하는 Bean 객체가 존재할지라도 자기 자신과 부모의 Context에 존재하면 찾을 수 없다. 자신이 먼저 그 다음 부모에 동일한 빈이 발견되면 자신이 가진것이 우선이 된다.

### 4. 웹 애플리케이션의 IoC 컨테이너 구성

- 웹 애플리케이션 안에 WebApplicationContext 타입의 IoC 컨테이너를 둔다.
- 몇 개의 서블릿이 중앙집중식으로 모든 요청을 다 받아서 처리하는 방식을 프론트 컨트롤러 패턴이라고 한다. 스프링은 이 패턴을 사용한다.
- 웹 요청을 한 번에 받을 수 있는 대표 서블릿을 등록해두고, 공통적인 선행 작업을 수행하게 한 후에, 각 요청의 기능을 담당하는 핸들러라고 불리는 클래스를 호출하는 방식이 일반적이다.

**웹 어플리케이션의 컨텍스트 구성 방법**

1. 서블릿 컨텍스트와 Root Application Context 계층구조
2. Root Application Context 단일구조
3. 서블릿 컨텍스트 단일구조

**Root Application Context 등록**

- 웹 애플리케이션의 시작과 종료 시 발생하는 이벤트를 처리하는 리스너인 ServletContextListener를 이용해 Root Web Application Context를 등록한다.
- 스프링은 ServletContextListener와 같은 기능을 가진 ContextLoaderListener를 제공한다.
- ContextLoaderListener : 웹 애플리케이션이 시작할 때 자동으로 루트 애플리케이션 컨텍스트를 만들고 초기화 해준다.

**서블릿 ApplicationContext 등록**

- 스프링의 웹 기능을 지원하는 프론트 컨트롤러 서블릿은 DisparcherServlet이다.
- DispatcherServlet은 서블릿이 초기화 될 떄 자신만의 컨텍스트를 생성하고 초기화한다.
- 동시에 웹 어플리케이션 레벨에 등록된 Root ApplicationContext를 찾아서 이를 자신의 부모 컨텍스트로 사용한다.
- 여러개의 DispatcherServlet 도 등록이 가능하며 네임스페이스로 구분한다.

### 5. IoC/DI를 위한 빈 설정 메타정보 작성

- **@Configuration** : 애플리케이션 컨텍스트가 사용할 설정정보

- **@Bean** : 오브젝트 생성을 담당하는 메소드라는 표시

```java
@Configuration
public class DaoFactory {
  @Bean //빈 이름: userDao, 빈 객체: UserDao
  public UserDao userDao() {
    return new UserDao(connectionMaker());
  }
  
  @Bean //빈 이름: connectionMaker, 빈 객체: DConnectionMaker
  public ConnectionMaker connectionMaker() {
    return new DConnectionMaker();
   }
}
```

```java
public class UserDaoTest {
  public static void main(String[] args) throws ClassNotFoundException, SQLException {
    ApplicationContext context = new AnnotationConfigApplicationContext(DaoFactory.class);
    UserDao dao = context.getBean("userDao", UserDao.class);
    ...
  }
}
```

<br>

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/ApplicationContext1.png)

@Configuration이 붙은 DaoFactory는 이 애플리케이션 컨텍스트가 활용하는 IoC 설정정보다. 애플리케이션 컨텍스트는 DaoFactory 클래스를 설정정보로 등록해두고 @Bean이 붙은 메소드의 이름을 가져와 빈 목록을 만들어둔다. 클라이언트가 애플리케이션 컨텍스트의 getBean() 메소드를 호출하면 자신의 빈 목록에서 요청한 이름이 있는지 찾고, 있다면 빈을 생성하는 메소드를 호출해서 오브젝트를 생성시킨 후 돌려준다.

### 6. 싱글톤 레지스트리와 오브젝트 스코프

DaoFactory를 직접 사용하는 것과 @Configuration 애노테이션을 추가해서 스프링의 애플리케이션 컨텍스트를 통해 사용하는 것은 결과만 보자면 동일한 것 같다. 하지만 스프링의 애플리케이션 컨텍스트는 기존 오브젝트 팩토리와는 중요한 차이점이 있다.

```java
DaoFactory factory = new DaoFactory();
UserDao dao1 = factory.userDao();
UserDao dao2 = factory.userDao();

System.out.println(dao1);
System.out.println(dao2);
```

두 개는 각기 다른 값을 가진 동일하지 않은 오브젝트다. userDao를 매번 호출하면 계속해서 새로운 오브젝트가 만들어질 것이다.

```java
ApplicationContext context = new AnnotationConfigApplication(DaoFactory.class);

UserDao dao3 = context.getBean("userDao", UserDao.class);
UserDao dao4 = context.getBean("userDao", UserDao.class);

System.out.println(dao3);
System.out.println(dao4);
```

두 오브젝트의 출력 값은 같으므로, getBean()을 두 번 호출해서 가져온 오브젝트가 동일하다는 사실을 알 수 있다. 스프링은 여러 번에 걸쳐 빈을 요청하더라고 매번 **동일한** 오브젝트를 돌려준다는 것이다. 단순하게 getBean()을 실행할 때마다 userDao() 메소드를 호출하고, 매번 new에 의해 새로운 UserDao가 만들어지지 않는다. 왜그럴까?

스프링은 기본적으로 별다른 설정을 하지 않으면 내부에서 생성하는 빈 오브젝트를 모두 **싱글톤**으로 만든다. 

> 왜 스프링은 싱글톤으로 빈을 만드는 것일까?

이는 스프링이 주로 적용되는 대상이 **자바 엔터프라이즈 기술을 사용하는 서버환경**이기 때문이다. 매번 클라이언트에서 요청이 올 때마다 오브젝트를 새로 만들어서 사용하게되면 서버에 부하가 걸릴 수 있다. 이렇게 애플리케이션 안에 제한된 수, 대개 한 개의 오브젝트만 만들어서 사용하는 것이 싱글톤 패턴의 원리다. 

- **Java로 구현한 싱글톤 패턴의 한계**

  - private 생성자를 갖고 있기 때문에 상속할 수 없다.

  - 싱글톤은 테스트하기 힘들다.

  - 서버환경에서는 싱글톤이 하나만 만들어지는 것을 보장하지 못한다.

  - 싱글톤의 사용은 전역 상태를 만들 수 있기 때문에 바람직하지 못하다.

- **싱글톤 레지스트리**

Java의 싱글톤 패턴의 구현 방식은 여러 가지 단점이 있기 때문에, **스프링은 싱글톤 형태의 오브젝트를 만들고 관리**하는 기능을 제공한다. 그것이 바로 싱글톤 레지스트리다. 싱글톤 레지스트리 덕분에 싱글톤 방식으로 사용될 애플리케이션 클래스라도 public 생성자를 가질 수 있으며 테스트하기도 편리해진다.

> 어떻게 스프링에서 내부적으로 싱글톤을 보장할까?

CGLIB라는 라이브러리를 사용해서 임의의 다른 클래스를 만들고, 그 클래스를 스프링 빈으로 등록한다. 바로 이 클래스가 싱글톤을 보장해준다. 즉 @Bean이 있는 메소드에 대해, 

1. 만약 스프링 컨테이너에 빈이 이미 등록되어 있으면 찾아서 반환
2. 스프링 컨테이너에 빈이 없으면 생성한 후 스프링 컨테이너에 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다.

스프링 컨테이너는 싱글톤 레지스트리이다. 따라서 스프링 빈이 싱글톤이 되도록 보장해주어야 하는데 @Configuration을 붙이면 CGLIB 기술을 사용하여 싱글톤을 보장하지만, @Bean만 적용할 경우 싱글톤 보장을 하지 못한다.

- **싱글톤과 오브젝트의 상태**

싱글톤은 멀티 쓰레드 환경이라면 여러 쓰레드가 동시에 접근해서 사용할 수 있다. 따라서 상태 관리에 주의를 기울여야 한다. 기본적으로 싱글톤은 상태 정보를 내부에 갖고 있지 않는 **무상태(stateless) 방식으로 만들어야 한다.**

상태가 없는 방식으로 클래스를 만드는 경우 요청에 대한 정보나, DB나 서버로부터 생성한 정보는 어떻게 다뤄야 할까? 파라미터와 로컬 변수, 리턴 값 등을 이용하면 된다. 메소드 파라미터나 메소드 안에서 생성되는 로컬 변수는 매번 새로운 값을 저장할 독립적인 공간이 만들어지기 때문에 싱글톤이라고 해도 여러 스레드가 변수의 값을 덮어쓸 일은 없다.

- **스프링 빈의 스코프**

스프링이 관리하는 오브젝트, 즉 **빈이 생성되고 존재하고 적용되는 범위**를 빈의 스코프라고 한다. 빈의 스코프는 싱글톤이다. 하지만 경우에 따라서는 싱글톤 외의 스코프를 가질 수 있다. 대표적으로 프로토타입 스코프가 있다. 그 외에도 웹을 통해 새로은 HTTP 요청이 올 때 생성되는 요청 스코프가 있고, 웹의 세션과 비슷한 세션 스코프도 있다.

### 7. 의존관계 주입(DI : Dependency Injection)

스프링 IoC 기능의 대표적인 동작원리는 의존관계 주입(DI)이라고 불린다. DI는 오브젝트 레퍼런스를 외부로부터 주입받고 이를 통해 여타 오브젝트와 다이내믹하게 의존관계가 만들어지는 것이 핵심이다.

의존관계는 정적인 클래스 의존 관계와, 실행 시점에 결정되는 동적인 객체(인스턴스) 의존 관계 둘을 분리해서 생각해야 한다.

- **정적인 클래스 의존관계**

클래스가 사용하는 import 코드만 보고 의존관계를 쉽게 파악할 수 있다. 애플리케이션을 실행하지 않고도 분석할 수 있다. 그런데 이러한 클래스 의존관계 만으로는 실제 어떤 객체가 주입 될지 알 수 없다.

- **동적인 객체 의존 관계(런타임 의존관계)**

애플리케이션 실행 시점(런타임)에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 실제 의존 관계가 연결 되는 것을 의존관계 주입이라 한다. 두 개의 클래스 또는 모듈이 의존관계에 있다고 말할 때는 항상 방향성을 부여해줘야 한다. 그림은 **A가 B에 의존하고 있음**을 나타낸다. 

> 그렇다면 의존하고 있다는 건 무슨 의미일까?
 
의존한다는 건 의존대상, 여기서는 **B가 변하면 그것이 A에 영향을 미친다**는 뜻이다. 반대로 B는 A에 의존하지 않는다. 의존하지 않는다는 말은 B는 A의 변화에 영향을 받지 않는다는 뜻이다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/Dependency%20relationship.png)

인터페이스를 통해 설계 시점에 느슨한 의존관계를 가지는 경우에는 런타임 시에 사용할 오브젝트가 어떤 클래스로 만든 것인지 미리 알 수가 없다. 프로그램이 시작되고 런타임 시에 의존관계를 맺는 대상, 즉 실제 사용대상인 오브젝트를 의존 오브젝트라고 말한다.

**의존관계 주입**은 이렇게 **구체적인 의존 오브젝트와 그것을 사용할 주체, 보통 클라이언트라고 부르는 오브젝트를 런타임 시에 연결해주는 작업**을 말한다. 정리하면 의존관계 주입이란 다음과 같은 조건을 충족하는 작업을 말한다.

- 클래스 모델이나 코드에는 런타임 시점의 의존관계가 드러나지 않는다. 그러기 위해서는 인터페이스에만 의존하고 있어야 한다.

- 런타임 시점의 의존관계는 컨테이너나 팩토리 같은 제3의 존재가 결정한다.

- 의존관계는 사용할 오브젝트에 대한 래퍼런스를 외부에서 주입해줌으로써 만들어진다.

> DI 받는다?
>
> DI의 동작방식은 외부로부터의 주입이다. 하지만 단지 외부에서 파라미터로 오브젝트를 넘겨주었다고 해서, DI가 아니라는 점이다. 주입받는 메소드 파라미터가 이미 특정 클래스 타입으로 고정되어 있다면 DI가 일어날 수 없다. DI에서 말하는 주입은 **다이내믹하게 구현 클래스를 결정해서 제공받을 수 있도록 인터페이스 타입의 파라미터를 통해** 이뤄져야 한다.

인터페이스를 사용함으로써 결합도가 낮아지고, 의존 대상이 바뀌어도 영향 받지 않고 변경을 통한 확장을 할 수 있다.

> 의존성 주입은 어떻게 할까?

**필드 주입**

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

**Setter 주입**

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

**생성자 주입**

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

### 8. XML을 이용한 설정

스프링의 애플리케이션 컨텍스트는 XML에 담긴 DI 정보를 활용할 수 있다. XML 설정은 @Configuration과 @Bean이 붙은 자바 클래스로 만든 설정과 내용이 동일하다. 하나의 @Bean 메소드를 통해 얻을 수 있는 빈의 DI 정보는 다음 세 가지다.

- 빈의 이름 : @Bean 메소드 이름이다. 이 이름은 getBean()에서 사용된다.

- 빈의 클래스 : 빈 오브젝트를 어떤 클래스를 이용해서 만들지 정의한다.

- 빈의 의존 오브젝트 : 빈의 생성자나 수정자 메소드를 통해 의존 오브젝트를 넣어준다.

|  | 자바 코드 설정정보 | XML 설정정보 | 
| :---------: | :----------------: | :---------: |
|   빈 설정파일    |   @Configuration   |    < beans >   |     
|  빈의 이름  |   @Bean methodName()    |   <bean id="methodName"   |      
|   빈의 클래스   |    return new BeanClass();    |    class="a.b.c... BeanClass">     |     

자바빈의 관례를 따라 수정자 메소드는 프로퍼티가 된다. 프로퍼티 이름은 메소드 이름에서 set을 제외한 나머지 부분을 사용하고, < property > 태그를 사용해 의존 오브젝트 관계를 정의한다. < property > 태그는 name과 ref라는 두 개의 애트리뷰트를 갖는다. name은 프로퍼티 이름이다. 이 프로퍼티 이름으로 수정자 메소드를 알 수 있다. ref는 수정자 메소드를 통해 주입해줄 오브젝트의 빈 이름이다.
  
