## String과 new String

Java에서 String 문자열 생성 방식은 두 가지가 있다.
두 방식의 차이는 **메모리 영역**이다.

```java
String a = new String("hello");
String b = "hello";
String c = "hello";
```

new를 통해 String을 생성하면 **Heap영역**에 새 객체가 생성되고, 리터럴(값)을 이용할 경우 **string constant pool**영역에 존재하게 된다. b와 c는 string constant pool에 만들어진 하나의 **동일한 객체**를 참조한다. 

```java
System.out.println(a.equals(b)); //true
System.out.println(a == b); //false : 문자열에서 ==은 주소값을 비교한다
System.out.println(b == c); //true
```
