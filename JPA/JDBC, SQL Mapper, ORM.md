## JDBC

- Java는 `DriverManager`를 통해 DB 종류와 상관없이 JDBC 표준 인터페이스를 구현하여 제공한다.
- `DriverManager`를 이용해 Connection 인스턴스를 얻고
- Statement를 이용해서 ResultSet를 얻어 조회하거나 사용

**JDBC의 한계**

- JDBC를 통해 여러 데이터 관련 로직을 처리할 경우 일정한 작업 흐름이 반복되는 코드가 생긴다. 
- 데이터 접근 계층이라면 데이터를 저장, 조회, 수정, 삭제라는 CRUD가 핵심 관심사이고 이와 관련된 로직에 의해서만 수정이 일어나야 한다. 하지만 JDBC를 사용할 경우 Connection을 얻고 Statement를 생성하는 등의 관심사도 존재하게 된다. 
- 또한 Java 코드내에 SQL문이 단순한 문자열로 포함되어있는데 자바 코드와는 달리 컴파일 체크가 불가하여 문법 오류의 확률이 높다.

## SQL Mapper

## Spring JDBC

```java
public class DAO {

  private JdbcTemplate jdbcTemplate;
 
  @Autowired
  public void setDataSource(DataSource dataSource){
    this.jdbcTemplate = new JdbcTemplate(dataSource);
  }
 
  public List<User> getUsers(){
    return jdbcTemplate.query("select * from users", new RowMapper<User>() {
      @Override
      public User mapRow(ResultSet rs, int rowNum) throws SQLException {
        return new User(rs.getInt("id"), rs.getString("name"));
      }
    });
  }
}
```

- JDBC에서는 기존의 Statement를 직접 바인딩 해주던 방식을 `JdbcTemplate`의 내부 메소드에서 처리해준다. 
- 기존에 ResultSet의 객체 매핑 과정에서 해당 객체로 변환하는 작업을 매번 작성하는 것이 아닌 `JdbcTemplate`이 알아서 결과를 바인딩해준다. 
- JDBC에서는 커넥션을 개발자가 직접 관리해주어야 해서 커넥션을 열고 닫는 작업을 해야했지만 `JdbcTemplate`은 하지 않아도 된다.
- 쿼리를 실행하는 내부에서 SQLException이 발생하는 경우 unchecked exception인 DataAccessException 타입 또는 그 하위 타입을 던져주면서 불필요한 코드를 제거할 수 있다.

SQL 쿼리문은 자바 코드와 같이 있으면 가독성이 떨어지는 단점과 단순히 SQL 쿼리문을 수정해야 하는 상황에서 자바 메소드 코드에 수정이 일어나면 관심사의 경계가 모호해진다.

## MyBatis

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

- SQL 쿼리문은 XML 파일에만 존재하고 자바 코드는 인터페이스에만 존재하여 쿼리문과 자바 코드를 따로 관리할 수 있다는 장점이 있다.
- 하지만 테이블의 필드가 추가되거나 삭제될 경우 이와 관련된 모든 DAO의 SQL 쿼리문과 객체의 필드 등의 수정이 일어난다.

## ORM(Object Relation Mapping)

- Object-relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
  
