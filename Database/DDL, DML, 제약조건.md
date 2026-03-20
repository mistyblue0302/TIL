## DDL - 테이블 변경, 제거

### ALTER TABLE : 이미 만든 테이블의 구조 변경

```sql
ALTER TABLE customers ADD COLUMN point INT DEFAULT 0; // 열 추가
ALTER TABLE customers MODIFY COLUMN address VARCHAR(500); // 열 데이터 타입 변경
ALTER TABLE customers DROP COLUMN point; // 열 삭제
```

ALTER TABLE은 유용한 기능이지만, 조심해서 사용해야 한다. 

수백만, 수천만 건의 데이터가 들어있는 거대한 테이블의 구조를 변경하는 작업은 엄청나게 많은 시간과 시스템 자원을 소모한다. 작업 중에는 테이블이 잠겨서 서비스가 일시적으로 멈출 수도 있다. 따라서 실무에서는 사용자가 적은 새벽 시간을 이용해 점검시간에 맞춰 작업하는 것이 일반적이다.

참고로 최신 버전의 MySQL에서는 열을 추가하는 것 정도는 실시간으로 적용해도 크게 문제가 되지는 않는다.

### DROP TABLE vs TRUNCATE TABLE

테이블을 다루다 보면 테이블의 내용을 비워야 할 때가 있다. 이때 `DROP`과 `TRUNCATE`는 비슷해 보이지만 아주 큰 차이가 있다.

- `DROP TABLE` : **테이블의 존재 자체를 삭제한다.**
  - `DROP TABLE orders;` 를 실행하면, `orders` 테이블의 모든 데이터는 물론, `orders`라는 테이블의 구조(설계도)까지 완전히 사라진다. 테이블을 다시 사용하려면 `CREATE TABLE`부터 다시 해야 한다. 마치 건물을 통
째로 철거하는 것과 같다.
- `TRUNCATE TABLE` : **테이블의 구조는 남기고, 내부 데이터만 모두 삭제한다.**
  - `TRUNCATE TABLE orders;` 를 실행하면, `orders`테이블 안의 모든 주문 데이터가 순식간에 사라진다. 하지만 `orders`라는 테이블의 구조(열, 제약조건 등)는 그대로 남아있어서, 바로 새로운 데이터를 `INSERT` 할 수 있다. 건물의 내부만 싹 비우고 뼈대는 그대로 두는 것과 같다.
  - **특징**
    - `DELETE FROM orders;` (WHERE 절 없는 DELETE)와 결과적으로는 같아 보이지만, `TRUNCATE`가 훨씬 빠르다. `DELETE`는 한 줄씩 지우면서 삭제 기록을 남기는 반면, `TRUNCATE`는 테이블을 초기화하는 개념이라 내부 처리 방식이 더 간단하고 빠르다.
    - `TRUNCATE`는 `AUTO_INCREMENT` 값도 초기화한다. 만약 `orders` 테이블에 1000개의 주문이 있어서 다음 `order_id`가 1001일 차례였다면, `TRUNCATE` 이후에는 다시 1부터 시작한다. (DELETE는 `AUTO_INCREMENT` 값을 초기화하지 않는다.)

이 둘의 차이를 명확히 이해하고 상황에 맞게 사용하는 것은 매우 중요하다. 테스트 데이터를 모두 지우고 처음부터 다시 시작하고 싶을 때는 `TRUNCATE`가 유용하고, 테이블 자체가 더 이상 필요 없을 때는 `DROP`을 사용한다.

**제약 조건 무시하기**

`products` 테이블은 `orders`에서 참조하고 있다. 이렇게 참조 당하는 테이블은 `DROP`, `TRUNCATE`를 할 수 없다.

```sql
DROP TABLE products;
```

```sql
Error Code: 3730. Cannot drop table 'products' referenced by a foreign key
constraint 'fk_orders_products' on table 'orders'.
```

- 실행 결과를 보면 `orders` 테이블에 있는 `fk_orders_products` 외래 키 제약 조건 때문에 테이블을 제거할 수 없다는 뜻이다.
- 만약 이 테이블을 제거할 수 있게 둔다면 `orders` 테이블 입장에서는 **주문된 상품의 정보를 찾을 수 없는(상품없는 주문)** 심각한 문제가 나타난다!

```sql
TRUNCATE TABLE products;
```

```sql
Error Code: 1701. Cannot truncate a table referenced in a foreign key
constraint (`my_shop`.`orders`, CONSTRAINT `fk_orders_products`)
```

- `TRUNCATE`의 경우도 마찬가지다. `TRUNCATE`는 내부의 데이터를 확인하지 않고, 모든 데이터를 빠르게 제거한다. 따라서 이 테이블의 모든 데이터를 제거한다면 `orders` 테이블 입장에서는 **주문된 상품의 정보를 찾을 수 없는(상품 없는 주문)** 심각한 문제가 발생할 수 있다.

만약 이런 외래 키 제약조건을 임시로 무시하고 싶다면 외래 키 체크를 비활성화 하면 된다.

```sql
SET FOREIGN_KEY_CHECKS = 0; -- 비활성화
SET FOREIGN_KEY_CHECKS = 1; -- 활성화
```

```sql
SET FOREIGN_KEY_CHECKS = 0; -- 비활성화
TRUNCATE TABLE products;
SET FOREIGN_KEY_CHECKS = 1; -- 활성화
```

**주의사항**

- `FOREIGN_KEY_CHECKS`를 비활성화하면 데이터 무결성이 깨질 수 있으므로, 필요한 작업을 마친 후에는 반드시 다시 활성화해야 한다.
- `SET XXX` 방식의 설정은 데이터베이스 접속이 연결되는 동안만 유효하다. 연결이 끊기면 설정이 사라진다. 따라서 다시 접속하는 경우 설정을 다시 해야 한다.

## DML - 등록

`INSERT`는 테이블에 새로운 데이터 행(row)을 추가하는 명령어다. 

AUTO_INCREMENT / DEFAULT 컬럼은 생략 가능

### INSERT 문법

INSERT 문의 기본 문법은 다음과 같다.

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

- `table_name` : 데이터를 추가할 테이블의 이름이다.
- `(column1, column2, ...)` : 값을 지정할 열의 목록이다. 이 목록을 생략하면 테이블의 모든 열에 순서대로 값을 넣어야 한다.
- `VALUES (value1, value2, ...)` : 지정된 각 열에 삽입될 값이다. 열 목록과 값 목록은 순서와 개수가 정확히 일치해야 한다.

> 열 목록을 사용하자

열 목록을 생략하고 `INSERT INTO products VALUES (NULL, '베이직 반팔 티셔츠', ...)`와 같이 값만 순서대로 나열하는 방법은 추천하지 않는다. 나중에 테이블 구조가 변경되면 기존의 `INSERT` 코드는 모두 오류를 일으키기 때문이다. 항상 데이터를 추가할 열의 목록을 명시적으로 작성하는 것이 안전하다. 물론 학습이나 테스트를 위한 것이라면 편의상 이런 방식을 사용해도 괜찮다!

## DML - 수정

`UPDATE` 는 이미 존재하는 데이터의 내용을 수정하는 명령어다.

### UPDATE 문법

UPDATE 문의 기본 문법은 다음과 같다.

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

- `table_name` : 데이터를 수정할 테이블의 이름이다.
- `SET column1 = value1, ...` : 수정할 열과 새로운 값이다. 쉼표(,)를 사용해 여러 열을 한 번에 수정할 수 있다.
- `WHERE condition` : 수정할 행을 식별하는 조건이다. 이 부분을 생략하면 테이블의 모든 행이 수정되므로 주의해야 한다.


<br>

[참고]

[https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C-%EC%8B%A4%EC%A0%84-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%9E%85%EB%AC%B8/dashboard?cid=338210](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C-%EC%8B%A4%EC%A0%84-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%9E%85%EB%AC%B8/dashboard?cid=338210)
