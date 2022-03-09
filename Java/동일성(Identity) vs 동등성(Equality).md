# 💡 **동일성(Identity) vs 동등성(Equality)**

> **동일성**은 주소가 같음을 의미하고, **동등성**은 객체가 같음을 의미한다. <br>
> 자바에서 ==는 동일성, equals는 동등성을 비교한다.

<br>

**Primitive Type**

Primitive Type은 객체가 아니기 때문에 동등성 비교를 할 수 없으며, 동일성 비교는 변수의 값으로 비교한다.

<br>

**Reference Type**

```java
String str1 = new String("Lee");
String str2 = new String("Lee");

System.out.println(str1 == str2); //false
System.out.println(str1.equals(str2)); //true
```

new 예약어를 통해 서로 다른 객체를 메모리에 할당하였으므로 **동일하지 않다.** <br>
하지만 두 객체의 내용은 같으므로 **동등하다.**

<br>

Object클래스의 equals()메소드는 다음과 같이 구현되어있다.

```java
public boolean equals(Object obj) {
        return (this == obj);
}
```

매개변수로 전달된 객체를 == 연산자로 비교하여 리턴한다. 즉 오버라이딩하지 않고 사용할 경우 동일성을 비교하게 된다. <br>
따라서 동등성 비교를 위해서는 equals 메소드를 Override 해야한다.

<br>

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

String 클래스는 equals()를 재정의 한다. ==를 통해 동일성을 판단하고, 동일하지 않다면 String인지 판단한 뒤 문자 하나하나를 비교한다. <br>
모든 문자가 같다면 동등하다고 판단한다.

<br>

**equals()와 hashcode()**

- equals() : 동등성 비교

- hashcode() : 동일성 비교

  equals()를 override할 때에는 hashcode()도 같이 override해야한다. 왜냐면 equals()비교를 통해 객체가 같아도 주소 값은 같지 않을 수도 있기 때문이다.


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

