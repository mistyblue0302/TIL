## Java 8 변경사항

### 1. Optional 클래스

Optional 클래스는 null 처리를 보다 간편하게 하기 위해서 만들어졌다. NullPointerException 발생을 명확하게 처리하려고 할 때에 사용한다.
    
```java
public final class Optional<T> extends Object
```

```java
List<String> name = getNames();
name.sort(); // names가 null이라면 NullPointerException이 발생함

if(name != null) {
   name.sort(); 
}
```

다음과 같은 경우 NullPointerException을 피하기 위해 null을 검사해주는 코드를 추가해야 한다. null 검사를 해야하는 변수가 많은 경우 코드가 복잡해질 수 있다.


**Optional 생성**

Optional 클래스가 비어 있는지 확인하는 메소드는 isPresent()이다.

```java
Obtional<String> emptyString = Obtional.empty(); //빈 객체 생성
System.out.println(emptyString);
System.out.println(emptyString.isPresent()); //false
```

데이터가 null이 아닌 경우 Optional.of()로 생성할 수 있다.
null을 저장하려고 하면 NullPointerException이 발생한다.

```java
Optional<String> commonString = Optional.of("JuSeon");
```

데이터가 null이 올 수도 있고 아닐 수도 있는 경우에는 ofNullable()메소드를 사용할 수 있다.

```java
String common = null;
Optional<String> nullableString = Optional.ofNullable(common);
```

### 2. 인터페이스의 Default Method
    
Default Method는 기본 구현을 가지는 메소드로 하위 호환성을 제공한다.

### 3. 날짜 관련 클래스 추가

Java 8에서는 java.time 패키지를 만들었다.

이전 : java.util.Date, Calendar
    
Java 8 : java.time.LocalDate

Date와 Calendar 클래스는 변할 수 있는 객체로 쓰레드에 안전하지 않았다. LocalDate 클래스는 불변 객체로 쓰레드레 안전하다. 

Java 8에 DayOfWeek 클래스가 추가되었다. MONDAY부터 SUNDAY까지의 상수가 enum에 선언되어 있다.

### 4. 병렬 배열 정렬

Java 8에서는 parallelSort()라는 정렬 메소드가 생겼다. 기존 sort()의 경우 단일 쓰레드로 수행되며, parallelSort()는 필요에 따라 여러 개의 쓰레드로 나뉘어 작업이 수행되므로 처리 속도가 더 빠르다.

### 5. StringJoiner 추가

순차적으로 나열되는 문자열을 처리할 때 사용한다.

## Java 8 추가된사항

### 1. 람다 표현식

람다 표현식은 매개 변수, 화살표 연산자, 처리식으로 구성되어 있다. 메소드가 하나만 존재하는 인터페이스는 @FunctionalInterface로 선언할 수 있으며, 이 인터페이스를 람다 표현식으로 처리할 수 있다.

### 2. Functional Interface(함수형 인터페이스)

추상 메소드가 하나인 인터페이스를 의미한다. default 메소드나 static 메소드는 여러 개 존재해도 상관 없다. 람다 표현식은 함수형 인터페이스로만 사용 가능하다.

**함수형 인터페이스 종류**

  - **Predicate**

    test()라는 메소드가 있으며, 두 개의 객체를 비교할 때 사용한다. 람다식으로는 T -> boolean 로 표현한다.

    ```java
    @FunctionalInterface
    public interface Predicate<T> {
      boolean test(T t);
    }
    ```

  - **Supplier**

    get()메소드가 있으며 아무런 인자를 받지 않고 리턴값은 generic으로 선언된 타입을 리턴한다. 람다식으로는 () -> T 로 표현한다.

    ```java
    @FunctionalInterface
    public interface Supplier<T> {
      T get();
    }
    ```

  - **Consumer**

    메소드 리턴값이 따로 없으며 출력을 할 때처럼 작업 수행 후 결과를 받을 일이 없을 때 사용한다. 람다식으로는 T -> void 로 표현한다.

    ```java
    @FunctionalInterface
    public interface Consumer<T> {
      void accept(T t);
    }
    ```

  - **Function**

    T는 입력 타입, R은 리턴 타입을 의미하며 변환을 할 필요가 있을 때에 사용한다.

    ```java
    @FunctionalInterface
    public interface Function<T, R> {
      R apply(T t);
    }
    ```

### 3. Stream

자바에서 스트림은 연속된 정보를 처리하는데 사용하며 다음과 같은 구조를 가진다.

```java
list.stream().filter(x-> x>10).count()
```

스트림 생성 : 컬렉션의 목록을 스트림 객체로 변환한다.

중개 연산 : 생성된 스트림 객체를 사용하여 연산 처리 한다. 반드시 있어야 하는 것은 아니다.

종단 연산 : 중개 연산에서 작업된 내용을 바탕으로 결과를 리턴한다.
    
