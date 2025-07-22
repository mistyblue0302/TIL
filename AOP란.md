## AOP

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface DistributedLock {
     ...
}
```

- @Target : AOP의 커스텀 애노테이션이 적용될 대상을 지정하는 enum값
  - FIELD, METHOD, PARAMETER 등이 있다.
- @Retention : 커스텀 애노테이션이 선언된 대상의 메모리를 언제까지 유지할 것인지 결정
  - SOURCE, CLASS, RUNTIME이 있다.
  - RetentionPolicy.SOURCE : 자바 컴파일러(javac)가 자바 코드를 컴파일 할 때, 해당 애노테이션의 메모리를 버림
  - RetentionPolicy.CLASS : 컴파일러가 해당 애노테이션의 메모리를 유지하여 컴파일하지만, JVM이 바이트코드를 해석해서 동작하는 Runtime단계에선 해당 메모리를 버린다.
  - RetentionPolicy.RUNTIME : JVM이 실제 클래스의 자바 바이트코드를 해석해서 동작하는 Runtime단계에서 메모리가 유지되고, Runtime이 종료되면 메모리도 사라진다.
