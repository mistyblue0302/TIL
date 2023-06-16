## JDBC, SQL Mapper, ORM

**데이터 접근 기술의 어려움**

각 DB마다 Connection 연결 방법, SQL 전달 방법, 응답 방법이 모두 다르기 때문에,

- 다른 DB로 변경하면 서버에 개발된 DB 코드도 함께 변경해야 하는 문제
- 개발자가 각각의 DB 마다 커넥션 연결, SQL 전달, 결과 응답 방법을 새로 작성해야 하는 문제가 있다.

## JDBC

Java는 `DriverManager`를 통해 DB 종류와 상관없이 JDBC 표준 인터페이스를 구현하여 제공한다. 

**JDBC의 한계**

- JDBC를 통해 여러 데이터 관련 로직을 처리할 경우 일정한 작업 흐름이 반복되는 코드가 생긴다. 
- 데이터 접근 계층이라면 데이터를 저장, 조회, 수정, 삭제라는 CRUD가 핵심 관심사이고 이와 관련된 로직에 의해서만 수정이 일어나야 한다. 하지만 JDBC를 사용할 경우 Connection을 얻고 Statements를 생성하는 등의 관심사도 존재하게 된다. 
- 또한 Java 코드내에 SQL문이 단순한 문자열로 포함되어있는데 자바 코드와는 달리 컴파일 체크가 불가하여 문법 오류의 확률이 높다.

## Spring JDBC

- JDBC에서는 기존의 Statement를 직접 바인딩 해주던 방식을 `JdbcTemplate`의 내부 메소드에서 처리해준다. 
- 기존에 ResultSet의 객체 매핑 과정에서 해당 객체로 변환하는 작업을 매번 작성하는 것이 아닌 `JdbcTemplate`이 알아서 결과를 바인딩해준다. 
- JDBC에서는 커넥션을 개발자가 직접 관리해주어야 해서 커넥션을 열고 닫는 작업을 해야했지만 `JdbcTemplate`은 하지 않아도 된다.
- 쿼리를 실행하는 내부에서 SQLException이 발생하는 경우 unchecked exception인 DataAccessException 타입 또는 그 하위 타입을 던져주면서 불필요한 코드를 제거할 수 있다.

SQL 쿼리문은 자바 코드와 같이 있으면 가독성이 떨어지는 단점과 단순히 SQL 쿼리문을 수정해야 하는 상황에서 자바 메소드 코드에 수정이 일어나면 관심사의 경계가 모호해진다.

## SQL Mapper

MyBatis는 자바 오브젝트와 SQL 사이의 자동 매핑 기능을 지원하는 프레임워크이다. 

```java
public interface UserMapper {
  User selectUser(Long id);
}
```

```xml
<mapper namespace="org.mybatis.example.UserMapper">
  <select id="selectUser" resultType="User">
    select * from user where id = #{id}
  </select>
```

이전과 비교하면 SQL 쿼리문은 XML 파일에만 존재하고 자바 코드는 인터페이스에만 존재하여 쿼리문과 자바 코드를 따로 관리할 수 있다는 장점이 있다. 하지만 테이블의 필드가 추가되거나 삭제될 경우 이와 관련된 모든 DAO의 SQL 쿼리문과 객체의 필드 등의 수정이 일어난다.

## ORM(Object Relation Mapping)

- ORM이란 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결)해주는 데이터 접근 기술
- MyBatis는 Java 코드와 SQL 쿼리문을 매핑 시켜주어야 했지만 JPA와 같은 ORM 기술은 객체가 DB에 연결되기 때문에, SQL을 직접 작성하지 않고 표준 인터페이스 기반으로 처리한다.
- 반복적인 SQL을 직접 작성할 필요 없이, 객체에 집중한 설계가 가능하다.
- ORM은 객체간의 관계를 바탕으로 SQL을 자동으로 생성하고 메소드를 통해 조작한다.
- 필드가 추가되거나 삭제될 시에 해당 객체만 수정하면 된다.


