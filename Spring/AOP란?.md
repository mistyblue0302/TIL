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
