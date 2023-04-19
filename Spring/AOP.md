## 6장. AOP

## AOP란?

AOP(Aspect Oriented Programming, 관점 지향 프로그래밍)란 여러 오브젝트에 나타나는 **공통적인 부가 기능**을 모듈화하여 **재사용**하는 기법이다.

## Spring의 AOP

Spring의 AOP는 내부적으로 다음과 같이 동작한다.

1. Dynamic Proxy 객체의 생성 요청
2. Pointcut을 통해 부가 기능 대상 여부 확인
3. Advice로 부가 기능 적용
4. 실제 기능 처리

### Advice

Advice는 어떤 부가기능을 **언제** 사용할지에 대한 정의

- @Before : 타겟 메소드가 호출되기 전
- @AfterReturning : 타겟 메소드가 성공적으로 결과값을 반환 후
- @AfterThrowing : 타겟 메소드가 수행 중 예외를 던지게 되면 수행
- @After : 타겟 메소드의 결과에 관계없이(성공, 예외 관계없이) 타겟 메소드가 완료되면
- @Around : 타겟 메소드 호출 전과 후

### JoinPoint

JoinPoint는 Advice가 적용될 수 있는 **위치**를 나타낸다.

### Pointcut

Pointcut은 Advice를 적용할 JoinPoint를 선별하는 작업을 말한다.

### Target

Target은 부가 기능이 적용될 대상이다.
### 1. 트랜잭션 코드의 분리

- 메소드 분리

```java
public void upgradeLevels() throws Exception {
    //트랜잭션 경계설정
    TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());
    try {
        //비즈니스 로직
        List<User> users = userDao.getAll();
        for (User user : users) {
            if (canUpgradeLevel(user)) {
                upgradeLevel(user);
            }
        }

        this.transactionManager.commit(status);
    } catch (Exception e) {
        this.transactionManager.rollback(status);
        throw e;
    }
}
```

트랜잭션 경계설정의 코드와 비즈니스 로직 코드 간에 서로 주고받는 정보가 없다. 따라서 이 두 가지 코드는 성격이 다를 뿐 아니라 서로 주고받는 것도 없는, 완벽하게 독립적인 코드다.

```java
public void upgradeLevels() throws Exception {
    TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());

    try {
        upgradeLevelsInternal();   
        this.transactionManager.commit(status);
    } catch (Exception e) {
        this.transactionManager.rollback(status);
        throw e;
    }
}

private void upgradeLevelsInternal() {
    List<User> users = userDao.getAll();
    for (User user : users) {
        if (canUpgradeLevel(user)) {
            upgradeLevel(user);
        }
    }
}

```
비즈니스 로직을 담당하는 코드를 메소드로 추출하여 독립시켰다. 사용자 레벨 업그레이드를 담당하는 비즈니스 로직 코드만 독립적인 메소드에 담겨 있기 때문에 수정하기에도 부담이 없고, 실수로 트랜잭션 코드를 건드릴 염려도 없어졌다.

### 2. DI를 이용한 클래스의 분리

- DI 적용을 이용한 트랜잭션 분리

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Transaction%20Separation.png)

지금 UserService는 UserServiceTest가 클라이언트가 되어 사용하고 있다. 이 둘은 직접 연결을 통해 강한 결합이 되어 있어 다른 무엇인가를 추가하기는 힘들다. 그래서 다음과 같이 UserService를 인터페이스로 만들고 UserService 인터페이스의 구현 클래스를 넣음으로써 클라이언트와 결합이 약해지고, 직접 구현 클래스에 의존하고 있지 않기 때문에 유연한 확장이 가능해진다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Transaction%20Separation1.png)

UserService를 구현한 또 다른 구현 클래스를 만든다. 이 클래스는 단지 트랜잭션의 경계설정이라는 책임을 맡고 있을 뿐이다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/Transaction%20Separation2.png)

- 분리된 트랜잭션 기능

```java
public class UserServiceTx implements UserService {

    UserService userService;
    PlatformTransactionManager transactionManager;

    public void setTransactionManager(PlatformTransactionManager transactionManager) {
        this.transactionManager = transactionManager;
    }

    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    @Override
    public void add(User user) {
        userService.add(user);
    }

    @Override
    public void upgradeLevels() {
        TransactionStatus status = this.transactionManager
                .getTransaction(new DefaultTransactionDefinition());
        try {
            userService.upgradeLevels();
            this.transactionManager.commit(status);
        } catch (RuntimeException e) {
            this.transactionManager.rollback(status);
            throw e;
        }
    }
}
```

- 트랜잭션 적용을 위한 DI 설정

![img4](https://github.com/dilmah0203/TIL/blob/main/Image/Transaction%20Separation3.png)

```xml
<bean id="userService" class="service.UserServiceTx">
    <property name="transactionManager" ref="transactionManager"/>
    <property name="userService" ref="userServiceImpl"/>
</bean>

<bean id="userServiceImpl" class="service.UserServiceImpl">
    <property name="userDao" ref="userDao"/>
    <property name="mailSender" ref="mailSender"/>
</bean>
```
이제 클라이언트는 UserServiceTx 빈을 호출해서 사용하도록 만들어야한다. 따라서 userService 라는 대표적인 빈 아이디는 UserServiceTx 클래스로 정의된 빈에게 부여해준다. userService 빈은 UserServiceImpl 클래스로 정의되는, 아이디가 userServiceImpl인 빈을 DI 하게 만든다.

- 트랜잭션 분리에 따른 테스트 수정

UserService 클래스가 인터페이스와 두 개의 클래스로 분리된 만큼 테스트에서도 적합한 타입과 빈을 사용하도록 변경해야 한다. @Autowired는 기본적으로 타입이 일치하는 빈을 찾아주는데, 같은 타입의 빈이 두 개라면 필드 이름을 이용해 빈을 찾는다.

- 트랜잭션 경계설정 코드 분리의 장점
  - 비즈니스 로직을 담당하고 있는 UserServiceImpl 코드를 작성할 때는 트랜잭션과 같은 기술적인 내용에는 전혀 신경쓰지 않아도 된다.
  - 비즈니스 로직에 대한 테스트를 손쉽게 만들어낼 수 있다.

