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

그래서 `UserService`를 인터페이스로 만들고 기존 코드는 `UserService` 인터페이스의 구현 클래스를 만들어넣도록 한다. 그러면 클라이언트와 결합이 약해지고, 직접 구현 클래스에 의존하고 있지 않기 때문에 유연한 확장이 가능해진다.




## AOP란?

AOP(Aspect Oriented Programming, 관점 지향 프로그래밍)란 여러 오브젝트에 나타나는 **공통적인 부가 기능**을 모듈화하여 **재사용**하는 기법이다.
















 

