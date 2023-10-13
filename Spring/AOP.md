## 6장. AOP

## 트랜잭션 코드의 분리

지금까지 만들었던 UserService의 메소드를 다시 봐보자.

```java
public class UserService {
	
	public void upgradeLevels() throws Exception {
        //트랜잭션 경계설정
		TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());
		try {
			List<User> users = userDao.getAll(); //비즈니스 로직 
			for (User user : users) {
				if (canUpgradeLevel(user)) {
					upgradeLevel(user);
				}
			}
            //트랜잭션 경계설정
			this.transactionManager.commit(status);
		} catch (RuntimeException e) {
			this.transactionManager.rollback(status);
			throw e;
		}
	}
}
```

얼핏 보면 트랜잭션 경계설정 코드와 비즈니스 로직 코드가 복잡하게 얽혀 있는듯이 보이지만, 자세히 살펴보면 두 가지 종료의 코드가 구분되어 있음을 알 수 있다. 비즈니스 로직 코드를 사이에 두고 트랜잭션 시작과 종료를 담당하는 코드가 앞뒤에 위치하고 있다. 비즈니스 로직을 담당하는 코드가 트랜잭션의 시작과 종료 작업 사이에 수행되어야 한다는 사항만 지켜지면 된다. 그렇다면 이 성격이 다른 코드를 두 개의 메소드로 분리할 수 있지 않을까?

```java
public class UserService {
	
	public void upgradeLevels() {
        //트랜잭션 경계설정
		TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());
		try {
			upgradeLevelsInternal(); //비즈니스 로직 호출
            //트랜잭션 경계설정
			this.transactionManager.commit(status);
		} catch (RuntimeException e) {
			this.transactionManager.rollback(status);
			throw e;
		}
	}

	//비즈니스 로직을 메소드로 분리
    private void upgradeLevelsInternal() {
        List<User> users = userDao.getAll();
        for (User user : users) {
            if (canUpgradeLevel(user)) {
                upgradeLevel(user);
            }
        }
    }
}
```

사용자 레벨 업그레이드를 담당하는 비즈니스 로직 코드만 독립적인 메소드에 담겨 있으니 이해하기도 편하고, 수정하기에도 부담이 없다. 그럼 트랜잭션 경계설정을 하는 기술적인 코드는 비즈니스 로직을 수행하는 UserService 클래스와 어울릴까? 간단하게 트랜잭션 코드를 클래스 밖으로 뽑아내면 된다.

## DI 적용을 이용한 트랜잭션 분리

지금 `UserService`는 `UserServiceTest`가 클라이언트가 되어서 사용하고 있다. 그런데 UserService는 현재 클래스로 되어 있으니 다른 코드에서 사용한다면 `UserService` 클래스를 직접 참조하게 되기 때문에 강한 결합도를 가진다. 트랜잭션 코드를 어떻게든 해서 `UserService` 밖으로 빼버리면 `UserService` 클래스를 직접 사용하는 클라이언트 코드에서는 트랜잭션 기능이 빠진 `UserService`를 사용하게 될 것이다. 구체적인 구현 클래스를 직접 참조하는 경우의 전형적인 단점이다. 

그래서 `UserService`를 인터페이스로 만들고 기존 코드는 `UserService` 인터페이스의 구현 클래스를 만들어넣도록 한다. 그러면 클라이언트와 결합이 약해지고, 직접 구현 클래스에 의존하고 있지 않기 때문에 유연한 확장이 가능해진다. `UserService`를 인터페이스로 하나 만들고 기존의 `UserService`의 이름을 `UserServiceImpl`이라는 `Userservice` 인터페이스를 구현한 하나의 클래스로 만들자. 그리고 트랜잭션 부분을 `UserService` 인터페이스를 구현한 또 다른 클래스 `UserServiceTx`로 만들자. `UserServiceTx`는 단지 트랜잭션의 경계설정이라는 책임을 맡고 있을 뿐이다. 그리고 스스로는 비즈니스 로직을 담고 있지 않기 때문에 또 다른 비즈니스 로직을 담고 있는 `UserService`의 구현 클래스에 실제적인 로직 처리 작업은 위임하는 것이다.

```java
public interface UserService {
	void add(User user);
	void upgradeLevels();
}
```

```java
public class UserServiceImpl implements UserService {

	public void upgradeLevels() {
		List<User> users = userDao.getAll();
		for (User user : users) {
			if (canUpgradeLevel(user)) {
				upgradeLevel(user);
			}
		}
	}
   }
```

```java
public class UserServiceTx implements UserService {

	UserService userService;
	PlatformTransactionManager transactionManager;

    	//메소드 구현과 위임
	public void add(User user) {
		this.userService.add(user);
	}

    	//메소드 구현
	public void upgradeLevels() {
        	//부가기능 수행
		TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());
		try {
            		//위임
			userService.upgradeLevels();

            		//부가기능 수행
			this.transactionManager.commit(status);
		} catch (RuntimeException e) {
			this.transactionManager.rollback(status);
			throw e;
		}
	}
}
```

## 트랜잭션 적용을 위한 DI 설정

클라이언트가 UserService라는 인터페이스를 통해 사용자 관리 로직을 이용하려고 할 때 먼저 트랜잭션을 담당하는 오브젝트가 사용돼서 트랜잭션에 관련된 작업을 진행해주고, 실제 사용자 관리 로직을 담은 오브젝트가 이후에 호출돼서 비즈니스 로직에 관련된 작업을 수행하도록 만든다.

이렇게 분리를 했을 때의 장점은 다음과 같다.

- 비즈니스 로직을 담당하고 있는 UserServiceImpl의 코드를 작성할 때는 트랜잭션과 같은 기술적인 내용에는 전혀 신경쓰지 않아도 된다. 
- 비즈니스 로직에 대한 테스트를 손쉽게 만들어낼 수 있다.

--- 

## 고립된 단위 테스트

가장 편하고 좋은 테스트 방법은 가능한 한 작은 단위로 쪼개서 테스트 하는 것이다. 작은 단위의 테스트가 좋은 이유는 테스트가 실패했을 때 그 원인을 찾기 쉽기 떄문이다.

하지만 UserServiceTest는 UserDao, TransactionManager, MailSender라는 세가지 의존관계를 갖고있다. 따라서 그 세 가지 의존관계를 갖는 오브젝트들이 테스트가 진행되는 동안에 같이 실행된다. UserService를 테스트하는 것처럼 보이지만 사실은 그 뒤에 존재하는 훨씬 더 많은 오브젝트와 환경, 서비스, 서버, 심지어 네트워크까지 함께 테스트하는 셈이 된다.

## 테스트 대상 오브젝트 고립시키기

그래서 테스트의 대상이 환경이나, 외부서버, 다른 클래스의 코드에 종속되고 영향을 받지 않도록 고립시킬 필요가 있다. 테스트를 의존 대상으로부터 분리해서 고립시키는 방법은 MailSender에 적용해봤던 대로 테스트를 위한 대역(테스트 스텁, 목 오브젝트)을 사용하는 것이다.

## 테스트를 위한 UserServiceImpl 고립

그래서 이럴 땐 테스트 대상인 UserServiceImpl과 그 협력 오브젝트인 UserDao에게 어떤 요청을 했는지를 확인하는 작업이 필요하다. 테스트 중에 DB에 결과가 반영되지는 않았지만, UserDao의 update() 메소드를 호출하는 것을 확인할 수 있다면, 결국 DB에 그 결과가 반영될 것이라고 결론을 내릴 수 있기 때문이다. UserDao와 같은 역할을 하면서 UserServiceImpl과의 사이에서 주고받은 정보를 저장해뒀다가, 테스트의 검증에 사용할 수 있게 하는 목 오브젝트를 만들 필요가 있다.

## 단위 테스트와 통합 테스트

단위 테스트의 단위는 정해진 게 없다. 메소드 하나를 단위로 볼 수도 있고, 클래스 하나를 단위로 볼 수도 있다.

이 책에서는 테스트 대상 클래스를 목 오브젝트 등의 테스트 대역을 이용해 의존 오브젝트나 외부의 리소스를 사용하지 않도록 고립시켜서 테스트 하는 것을 **단위 테스트**라고 부르고, 두 개 이상의 성격이나 계층이 다른 오브젝트가 연동하도록 만들어 테스트하거나, 외부의 DB나 파일, 서비스 등의 리소스가 참여하는 테스트틑 **통합 테스트**라고 부르겠다.

통합 테스트란 두 개 이상의 단위가 결합해서 동작하면서 테스트가 수행되는 것이라고 보면 된다. 예를들면, 스프링의 컨텍스트 프레임워크를 이용해서 컨텍스트에서 생성되고 DI된 오브젝트를 테스트하는 것도 통합 테스트이다.

통합 테스트와 단위 테스트 중 어떤 방법을 쓸지는 다음 목록을 참고하자.

- 항상 단위 테스트를 먼저 고려한다.
- 단위 테스트는 테스트 대역을 사용하여 테스트를 만들면 실행속도도 빠르고 테스트 대상 외의 환경 또는 코드로 영향을 받지 않는다.
- 외부 리소스를 사용해야만 가능한 테스트는 통합 테스트로 만든다.
- DAO 테스트는 DB라는 외부 리소스를 사용하기 때문에 통합 테스트로 분류된다.
- DAO를 이용하는 코드를 테스트 대역을 이용하여 테스트하도록 할 수 있다.
- 의존관계가 여러개라면 통합 테스트는 필요하다. 하지만, 단위 테스트를 충분히 거쳤다면 통합 테스트의 부담은 줄어든다.
- 스프링 테스트 컨텍스트 프레임워크를 이용하는 테스트는 통합 테스트다.

---

## 목 프레임워크

**단위 테스트를 만들기 위해서는 스텁이나 목 오브젝트의 사용이 필수적이다. 의존관계가 없는 단순한 클래스나 세부 로직을 검증하기 위해 메소드 단위로 테스트할 때가 아니라면, 대부분 의존 오브젝트를 필요로 하는 코드를 테스트하게 되기 때문이다.**

목 오브젝트를 만드는 일은 번거롭다는 점이 문제다. 그러나 이런 번거로운 목 오브젝트를 편리하게 작성하도록 도와주는 다양한 목 오브젝트 지원 프레임워크가 있다.

## Mockito 프레임워크

Mockito 프레임워크는 목 오브젝트를 편리하게 작성하도록 도와준다.

```java
UserDao mockUserDao = mock(UserDao.class);
```

이렇게 만들어진 목 오브젝트는 아직 아무런 기능이 없다. 여기에 만약 getAll() 메소드를 불러올 때 사용자 목록을 리턴하도록 스텁 기능을 추가하고 싶다면 다음과 같이 추가하면 된다.

```java
when(mockUserDao.getAll()).thenReturn(this.users);
```

`mockUserDao.getAll()`이 호출되었을 때(when), users 리스트를 리턴해주어라(thenReturn)는 선언이다. 이렇게 정의한 후에 mockUserDao.getAll()이 호출되면 users가 리턴이 될 것이다.

그럼 리턴타입이 없는 update() 또는 insert()의 기능을 하는 메소드를 호출하는 경우는 어떻게 검증을 할까?

```java
verify(mockUserDao, time(2)).update(any(User.class));
```

위 코드는 User 타입의 오브젝트를 파라미터로 받으며, update() 메소드가 두 번 호출되었는지 (times(2)) 확인(verify)하라는 것이다.

- Mockito 목 오브젝트 단계
  - 인터페이스를 이용해 목 오브젝트를 만든다.
  - 목 오브젝트가 리턴할 값이 있으면 이를 지정해준다. 메소드가 호출되면 예외를 강제로 던지게 만들 수도 있다.
  - 테스트 대상 오브젝트에 DI 해서 목 오브젝트가 테스트 중에 사용되도록 만든다.
  - 테스트 대상 오브젝트를 사용한 후에 목 오브젝트의 특정 메소드가 호출됐는지, 어떤 값을 가지고 몇 번 호출됐는지를 검증한다.

목 오브젝트를 통해 변경된 테스트 코드는 다음과 같다.

```java
@Test
public void mockUpgradeLevels() throws Exception {
    UserServiceImpl userServiceImpl = new UserServiceImpl();

    //다이내믹한 목 오브젝트 생성과 메소드의 리턴 값 설정, DI까지 세 줄이면 충분하다.
    UserDao mockUserDao = mock(UserDao.class);	    
    when(mockUserDao.getAll()).thenReturn(this.users);
    userServiceImpl.setUserDao(mockUserDao);

    //리턴타입이 없는 메소드를 가진 목 오브젝트는 더욱 간단하게 만들 수 있다.
    MailSender mockMailSender = mock(MailSender.class);  
    userServiceImpl.setMailSender(mockMailSender);

    userServiceImpl.upgradeLevels();

    //목 오브젝트가 제공하는 검증기능을 통해 어떤 메소드가 몇번 호출되었는지, 파라미터는 무엇인지 확인 가능하다. 
    verify(mockUserDao, times(2)).update(any(User.class));				  
    verify(mockUserDao, times(2)).update(any(User.class));
    verify(mockUserDao).update(users.get(1));
    assertThat(users.get(1).getLevel(), is(Level.SILVER));
    verify(mockUserDao).update(users.get(3));
    assertThat(users.get(3).getLevel(), is(Level.GOLD));

    ArgumentCaptor<SimpleMailMessage> mailMessageArg = ArgumentCaptor.forClass(SimpleMailMessage.class);  
    verify(mockMailSender, times(2)).send(mailMessageArg.capture());
    List<SimpleMailMessage> mailMessages = mailMessageArg.getAllValues();
    assertThat(mailMessages.get(0).getTo()[0], is(users.get(1).getEmail()));
    assertThat(mailMessages.get(1).getTo()[0], is(users.get(3).getEmail()));
}
```

- times() : 메소드 호출 횟수 검증
- any() : 파라미터의 내용은 무시하고 호출 횟수만 확인, 호출 횟수 검사가 끝나면 목 오브젝트가 호출됐을 때의 파라미터를 하나씩 검증


## 다이내믹 프록시와 팩토리 빈















 

