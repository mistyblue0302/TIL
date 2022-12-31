## 동일성(Identity) vs 동등성(Equality)

**동일성**은 두 객체가 할당된 **메모리 주소가 같음**을 의미하고, **동등성**은 두 개의 객체가 **같은 정보를 가지고 있음**을 뜻한다. 동등성은 변수가 참조하고 있는 객체의 주소가 서로 다르더라도 내용만 같으면 두 변수는 동등하다고 할 수 있다. 자바에서 **==는 동일성**, **equals는 동등성**을 비교한다.

### Primitive Type

Primitive Type은 객체가 아니기 때문에 주소가 없어 동등성 비교를 할 수 없으며, 동일성 비교는 변수의 값으로 비교한다.
객체가 아니라 주소가 없으므로 == 연산자를 사용하였을 때 내용이 같으면 동일하다고 말한다.

### Reference Type

```java
String str1 = new String("Hello");
String str2 = "Hello";
String str3 = "Hello";

System.out.println(str1.equals(str2));  // true
System.out.println(str1 == str2);       // false
System.out.println(str2 == str3);       // true
```

new를 통해 Heap 영역에 생성된 String str1과 리터럴(값)을 이용해 String constant Pool 영역에 위치한 String str2은 **동일하지 않다.** 하지만 두 객체의 내용은 같으므로 **동등하다.**

Object클래스의 equals()메소드는 다음과 같이 구현되어있다.

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

매개변수로 전달된 객체를 == 연산자로 비교하여 리턴한다. 즉 오버라이딩하지 않고 사용할 경우 동일성을 비교하게 된다. 따라서 동등성 비교를 위해서는 equals 메소드를 Override 해야한다.

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
    }
```

String 클래스는 ==를 통해 동일성을 판단하고, 동일하지 않다면 String인지 판단한 뒤 문자 하나하나를 비교한다. 모든 문자가 같다면 동등하다고 판단한다.

### equals()와 hashcode()

equals()를 override할 때에는 hashcode()도 같이 override해야한다. 그렇지 않으면 hashcode()를 활용하는 Collection을 이용할 때 문제가 발생한다. hashcode()를 사용하는 Java의 Collection은 HashMap, HashTable 등이 있다. 공통적인 기능으로는 **hashcode()를 사용하여 키 값을 결정**한다는 것이다. 여기서 키란 객체를 판별하는 수단으로, 객체의 hashcode()가 다르다면 다른 객체라고 판단하기 때문에 동등한 객체로 판단하기 위해서는 hashcode()도 재정의해야하는 것이다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/HashMap.png)

HashMap의 get 메소드를 보면 Key값으로 Node를 get할 때 앞에서 잠깐 살펴본 Object 클래스의 hashCode()를 이용하여 Node를 get해옴을 알 수 있다. 문제는, equals()를 오버라이딩하여 두 객체가 동등하다면, hashCode 값도 항상 동일해야만 한다는 점이다. 만일 그렇지 않다면, 예를 들어 HashMap의 Key로 어떤 객체를 사용할 때, 내용이 같은 객체(동등성을 가진)를 생성하여 Key로 넣더라도 HashCode값이 다르기 때문에 원하는 Value를 가져오지 못하기 때문이다.

```java
public class Example {
   private int number;

    public Example(int number){
       this.number = number;
   }

    @Override
    public boolean equals(Object o) {
        if (this == o) //주소가 같으면 true
            return true;
        if (o == null || getClass() != o.getClass()) //obj가 null이거나 같은 자료형이 아니면 false
            return false;
        Example example = (Example)o; //같은 클래스이므로 형 변환
        return Objects.equals(number, example.number);
    }

    @Override
    public int hashCode() {
        return Objects.hash(number);
    }
}
```

```java
public class Main {
    public static void main(String[] args){
        Example ex = new Example(32);
        Example ex2 = new Example(32);

        System.out.println("hashcode : "+ex.hashCode());
        System.out.println("hashcode : "+ex2.hashCode());
        System.out.println("equals : "+ ex.equals(ex2));
    }
}
```

