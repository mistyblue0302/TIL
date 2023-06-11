## 동일성(Identity) vs 동등성(Equality)

- **동일성**은 할당된 **메모리 주소가 같음**을 의미하고, **동등성**은 두 개의 객체가 **같은 내용을 가지고 있음**을 뜻한다. 
- 동등성은 변수가 참조하고 있는 객체의 주소가 서로 다르더라도 내용만 같으면 두 변수는 동등하다고 할 수 있다. 
- 자바에서 `==`는 **동일성**, `equals`는 **동등성**을 비교한다.

### Primitive Type

- Primitive Type은 객체가 아니기 때문에 객체 타입에서 지원하는 동등성 비교를 할 수 없으며, 동일성 비교는 변수의 값으로 비교한다. 
- `==` 연산자를 사용하였을 때 내용이 같으면 동일하다.

### Reference Type

- new를 통해 Heap 영역에 생성된 String str1과 리터럴(값)을 이용해 String constant Pool 영역에 위치한 String str2은 **동일하지 않다.** 
- 하지만 두 객체의 내용은 같으므로 **동등하다.**

```java
String str1 = new String("Hello");
String str2 = "Hello";
String str3 = "Hello";

System.out.println(str1.equals(str2));  // true
System.out.println(str1 == str2);       // false
System.out.println(str2 == str3);       // true
```

아래의 코드를 보자.

```java
public class PhoneNumber {

    int number1;
    int number2;
    int number3;

    public PhoneNumber(int number1, int number2, int number3) {
        this.number1 = number1;
        this.number2 = number2;
        this.number3 = number3;
    }

    public static void main(String[] args) {
        Map<PhoneNumber, String> map = new HashMap<>();
        map.put(new PhoneNumber(1, 2, 3), "juseon");
        System.out.println(map.get(new PhoneNumber(1, 2, 3)));
    }
}
```

위 코드의 결과는 null이 나오게 된다. 이유는 무엇일까?

위와 같이 `equals()`, `hashCode()`를 재정의하지 않으면 Object 클래스의 메소드를 사용하게 된다.
        
```java
public class Object {

    public native int hashCode();

    public boolean equals(Object obj) {
        return (this == obj);
    }
}
```

Object 클래스의 메소드는 위와 같다.
- `hashCode()`를 보면 native가 붙어있는데 JVM이 내부적으로 메모리 주소를 이용해서 해시코드를 만든다.
- `equals()` 메소드는 매개변수로 전달된 객체를 `==` 연산자로 비교하여 리턴한다. 즉, 주소값이 같으면 true, 다르면 false를 반환하게 된다.

`HashMap`, `HashTable`, `HashSet`, `ConcurrentHashMap`에서는 `equals()`, `hashCode()`를 사용하기 때문에 재정의를 하지 않았다면 위와 같은 문제가 발생할 수 있다. 같은 객체임에도 key를 인식하지 못하고 null을 반환하는 문제다.

### equals()와 hashcode()

```java
public class PhoneNumber {

    int number1;
    int number2;
    int number3;

    public PhoneNumber(int number1, int number2, int number3) {
        this.number1 = number1;
        this.number2 = number2;
        this.number3 = number3;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        
        PhoneNumber phoneNumber = (PhoneNumber) o;
        return number1 == phoneNumber.number1 && number2 == phoneNumber.number2 && number3 == phoneNumber.number3;
    }

    @Override
    public int hashCode() {
        return Objects.hash(number1, number2, number3);
    }

    public static void main(String[] args) {
        Map<PhoneNumber, String> m = new HashMap<>();
        m.put(new PhoneNumber(1, 2, 3), "juseon");
        System.out.println(m.get(new PhoneNumber(1, 2, 3)));
    }
}
```

만약 `equals()`만 재정의하고 `hashCode()`를 재정의하지 않으면 `hashCode()`는 Object 클래스 메소드를 사용해 다른 값이 나올 것이다. 즉, 논리적으로 같다고 정의한 객체임에도 불구하고 해시코드 값이 달라 다른 버킷에 저장될 수 있다. 그렇기 때문에 `equals()`를 재정의하려면 `hashCode()`도 재정의해야한다.
