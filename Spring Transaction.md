## 트랜잭션(Transaction)

**트랜잭션**이란, 깨져서는 안되는 하나의 작업 단위를 의미한다. 그래서 트랜잭션은 commit(작업이 마무리 됨)으로 성공 하거나 rollback(작업을 취소하고 이전의 상태로 되돌림)으로 실패 이후 취소되어야 한다. DB에서는 트랜잭션을 조작함으로써 DB에 대한 **완전성**을 보장한다. 즉, 작업을 모두 완벽하게 처리하거나 또는 처리하지 못할 경우에는 원래 상태로 복구해서 작업의 일부만 적용되는 현상이 발생하지 않게 한다.

##  Spring이 제공하는 트랜잭션 기술

**JDBC api에서의 트랜잭션**

```java
public void upgradeLevels() throws Exception {
  //(1) DB Connection 생성
  //(2) 트랜잭션 시작
  try {
    //(3) DAO 메소드 호출
    //(4) 트랜잭션 커밋
  }
  catch(Exception e) {
    //(5) 트랜잭션 롤백
    throw e;
  }
  finally {
    //(6) DB Connection 종료
  }
}
```

문제점

- 깔끔하던 Service 코드가 복잡해짐
- 데이터 엑세스 기술에 의존적인 코드
- 비즈니스 로직과는 다른 관심사의 일을 수행

Spring은 다음 세 가지 기술을 이용하여 위의 문제점을 해결해준다.

- 트랜잭션 동기화
- 트랜잭션 추상화
- AOP를 이용한 트랜잭션(Transaction) 분리

### 1. 트랜잭션 동기화(Transaction Synchronization)

JDBC를 이용하는 개발자가 직접 여러 개의 작업을 하나의 트랜잭션으로 관리하려면 Connection 객체를 공유하는 등 상당히 불필요한 작업들이 많이 생길 것이다. Spring은 이러한 문제를 해결하기 위해 트랜잭션 동기화를 제공한다. **트랜잭션 동기화란** 트랜잭션을 시작하기 위한 Connection 객체를 특별한 저장소에 보관해두고 필요할 때 꺼내쓸 수 있도록 하는 기술이다.

```java
//동기화 시작
TransactionSynchronizeManager.initSynchronization(); //트랜잭션 동기화 작업을 초기화
Connection c = DataSourceUtils.getConnection(dataSource); //DB Connection 생성 및 트랜잭션 시작

//작업 진행

//동기화 종료
DataSourceUtils.releaseConnection(c, dataSource);
TransactionSynchronizeManager.unbindResource(dataSource); 
TransactionSynchronizeManager.clearSynchronization(); 
```

트랜잭션은 하나의 Connection에 종속되기 때문에, 다수의 DB를 사용할 때 트랜잭션으로 경계설정이 불가능해진다. 예를들어, Hibernate에서는 Connection이 아닌 Session이라는 객체를 사용하기 때문이다. 아직 **데이터 접근 기술에 의존적**이다.

### 2. 트랜잭션 추상화

위 문제가 발생하는 이유는 데이터 접근 기술마다 DB와의 연결 방법이 다르기 때문이다. 하지만 가져오는 트랜잭션 객체만 다를 뿐 구현 방식에 상관 없이 동일한 임무를 수행하는 구현체들에 대한 추상화가 가능하다. Spring은 **PlatformTransactionManager**라는 인터페이스를 사용해 각 구현체들이 트랜잭션을 가져오는 방식을 추상화하였다. 따라서 종속적인 코드를 이용하지 않고도 **일관되게 트랜잭션을 처리**할 수 있다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Platformtransactionmanager.png)

하지만 트랜잭션 관리 코드들과 비즈니스 로직 코드와 결합되어 두 가지 책임을 가지는 문제가 있다.

### 3. AOP를 이용한 트랜잭션(Transaction) 분리

Spring은 선언적 트랜잭션 방식인 @Transactional 애노테이션을 이용하여 트랜잭션을 생성, 종료하는 일을 비즈니스 로직과 분리하게 도와준다.


```java
public void addUsers(List<User> userList) {

        //트랜잭션의 설정정보를 꺼내옴(트랜잭션 구동에 필요한 객체)
	TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());
	
	try {
		for (User user: userList) {
			if(isEmailNotDuplicated(user.getEmail())){
				userRepository.save(user);
			}
		}

		this.transactionManager.commit(status);
	} catch (Exception e) {
		this.transactionManager.rollback(status);
		throw e
	}
}
```

위 코드는 트랜잭션 정보를 가져올 뿐만 아니라 비즈니스 로직과 결합되어 있으므로 분리하는 것이 적합하다. Spring은 트랜잭션 코드와 같은 부가 기능 코드를 클래스 밖으로 빼내어 별도의 모듈로 만드는 AOP를 적용한 트랜잭션 애노테이션 @Transactional을 지원한다. 그럼 다음과 같이 비즈니스 로직만 남길 수 있다. 

```java
@Service
@RequiredArgsConstructor
@Transactional
public class UserService {

    private final UserRepository userRepository;

    public void addUsers(List<User> userList) {
        for (User user : userList) {
            if (isEmailNotDuplicated(user.getEmail())) {
                userRepository.save(user);
            }
        }
    }
}
```

### Spring 트랜잭션 속성

Spring은 트랜잭션 경계설정 뒤에 전파(propagation), 고립(ioslation), 읽기전용(read-only), 제한시간(timeout)이라는 속성을 지정할 수 있다.

- propagation : 트랜잭션의 경계에서 이미 진행 중인 트랜잭션이 있을 때 어떻게 동작할지 결정
  - REQUIRED : 기본 속성, 트랜잭션이 있으면 참여하고 없으면 새로운 트랜잭션을 생성
  - REQUIRES_NEW : 항상 새로운 트랜잭션을 시작하고, 진행 중인 트랜잭션이 있다면 보류시키고 자신을 먼저 실행
  - MANDATORY : 트랜잭션이 있으면 참여하고 없으면 예외 발생
  - SUPPORTS : 트랜잭션이 있으면 참여하고 그렇지 않으면 트랜잭션 없이 진행
  - NESTED : 이미 진행 중인 트랜잭션이 있으면 중첩 트랜잭션을 시작한다. 부모 트랜잭션 커밋, 롤백엔 영향을 받지만 자신의 커밋, 롤백은 부모 트랜잭션에 영향을 주지 못함
  - NEVER : 트랜잭션을 사용하지 않는다. 트랜잭션이 존재하면 예외 발생
  - NOT_SUPPROTED : 이미 실행중인 트랜잭션이 있다면 보류시키고 트랜잭션을 사용하지 않는다. 
  
- isolation : 여러 트랜잭션이 진행될 때 트랜잭션의 작업 결과를 타 트랜잭션에게 어떻게 노출할지 결정
  - DEFAULT : 사용하는 DB 드라이버의 기본 설정
  - READ_UNCOMMITTED : 커밋하지 않은 정보는 다른 트랜잭션이 읽을 수 있음
  - READ_COMMITTED : 커밋되지 않은 정보는 다른 트랜잭션이 읽을 수 없음
  - REPEATABLE_READ : 다른 트랜잭션이 읽은 정보를 수정할 수 없음
  - SERIALIZABLE : 트랜잭션을 순차적으로 진행

- readOnly : 쓰기 작업이 일어나는 것을 의도적으로 방지

- timeout : 트랜잭션을 수행하는 제한 시간 설정

<br>

참고

[우아한Tech 트랜잭션](https://www.youtube.com/watch?v=aX9c7z9l_u8)
