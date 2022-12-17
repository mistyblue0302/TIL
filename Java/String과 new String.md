## String과 new String

Java에서 String 문자열 생성 방식은 두 가지가 있다.
두 방식의 차이는 **메모리 영역**이다.

```java
String a = new String("hello");
String b = "hello";
String c = "hello";
```

new연산자를 통해 String을 생성하면 **Heap영역**에 존재하게 되고, 리터럴(값)을 이용할 경우 **string constant pool**영역에 존재하게 된다.
b와 c는 같은 객체를 바라보지만, a는 다른 메모리의 객체를 바라보게 된다.

```java
System.out.println(a.equals(b)); //true
System.out.println(a == b); //false : 문자열에서 ==은 주소값을 비교한다
System.out.println(b == c); //true
```

- **Heap** 
  - 자바의 인스턴스(new 명령을 통해 생성된)와 배열이 할당되는 곳으로, **런타임 데이터**를 저장하는 영역이다. Heap영역에 보관되는 메모리는 메소드 호출이 끝나도 소멸되지 않는다.

- **Stack** 
  - **지역변수와 매개변수**가 저장되는 곳으로, 프로그램 실행 과정에서 임시로 할당되며 메소드 호출이 끝나면 스택에서 제거된다.

- **string constant pool**
  - Heap영역으로, 객체를 재사용하기 위해 만들어졌다.

