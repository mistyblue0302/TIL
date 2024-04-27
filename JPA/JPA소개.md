## JPA란

- Java Persistence API
- 자바 진영의 ORM 기술 표준

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/JPA.png)

- **JPA**는 **자바 진영의 ORM 기술 표준**으로 애플리케이션과 JDBC 사이에서 동작
- **패러다임 불일치 문제를 개발자 대신 해결해준다.**
  - 객체는 다형성, 추상화, 상속의 성질을 갖고 있으며 연관관계를 참조를 이용한다. 반면 데이터베이스는 데이터 중심으로 구조화되어 있고 연관관계를 외래키를 이용해 참조한다. 이러한 패러다임 불일치는 개발자가 중간에서 해결해 주어야하는 단점이 있다.
- 객체를 데이터베이스에 저장할 때
  - JPA는 Entity 분석 및
  - INSERT 쿼리문을 만들고
  - JDBC API를 사용해 쿼리를 DB에 보내준다.

## JPA를 왜 사용해야 할까?

### 생산성

- 저장: jpa.persist(member)
- 조회: Member member = jpa.find(memberId)
- 수정: member.setName(“변경할 이름”)
- 삭제: jpa.remove(member)

CRUD가 다 정의되어 있어서 편하다.

### 유지보수성

SQL에 의존적인 개발을 할 경우 엔티티 필드에 수정이 일어날 경우 Mapper 부터 JDBC API 코드를 변경해야한다. 반면 JPA는 이러한 과정을 대신 처리해주기 때문에 유지보수해야 할 코드가 줄어든다.

## JPA의 성능 최적화 기능

1. 1차 캐시와 동일성(identity) 보장
2. 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
3. 지연 로딩(Lazy Loading)

### 1차 캐시와 동일성 보장

- 같은 트랜잭션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상
- DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read 보장
  
~~~ java
String memberId = "100";
Member m1 = jpa.find(Member.class, memberId); //SQL
Member m2 = jpa.find(Member.class, memberId); //캐시

println(m1 == m2);  // true, SQL 1번만 실행
~~~

### 트랜잭션을 지원하는 쓰기 지연 - INSERT

- 트랜잭션을 커밋할 때까지 INSERT SQL을 모음
- JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송

~~~ java
transaction.begin(); // [트랜잭션] 시작
 
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.

//커밋하는 순간 데이터베이스에 INSERT SQL을 모아서 보낸다.
transaction.commit(); // [트랜잭션] 커밋
~~~

### 지연 로딩과 즉시 로딩

- 지연 로딩: 객체가 실제 사용될 때 로딩
- 즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/laze%20loading.png)

지연 로딩은 member를 조회 했을 때, member만 가져온다. 그리고 3번째 줄에서 team.getName을 했을 때, jpa가 db에 team에 대한 query를 날려서 가져온다. 즉,필요한 시점에 쿼리를 날려 값을 가져온다.

즉시 로딩은 member를 조회 했을 때, member와 연관된 객체까지 모두 가져온다.
