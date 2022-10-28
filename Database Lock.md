## Database Lock

> 데이터베이스에 Lock이 왜 필요할까?

트랜잭션들이 **동시에** 수행될 때, **일관성**을 해치지 않도록 데이터 접근을 제어한다.

### Lock의 종류

동시성 제어는 어떻게 할까? Lock에는 크게 낙관적 잠금(Optimisstic Lock)과 비관적 잠금(Pessimistic Lock)이 있다.

- 낙관적 잠금 
    - 데이터 갱신 시 **경합이 발생하지 않을 것**이라고 봄
    - 동시 업데이트가 거의 없는 경우 
- 비관적 잠금
    - 데이터를 **동시에 수정 할 가능성이 높다**고 봄
    - 동시 업데이트가 빈번한 경우

### 비관적 잠금 연산의 종류

1. 공용(shared) lock

read 연산 실행 가능, write 연산 실행 불가능
데이터에 대한 사용권을 여러 트랜잭션이 함께 가질 수 있다.

2. 베타(exclusive) lock

read연산과 write 연산 모두 실행 가능
베타 lock 연산을 실행한 트랜잭션만 해당 데이터에 대한 독점권을 가짐

### Lock의 단위

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Unit%20of%20lock.png)

### Lock으로 발생할 수 있는 문제점

1. 블로킹

2. 데드락