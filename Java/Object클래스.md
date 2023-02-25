## Object클래스

자바에서 모든 클래스의 부모 클래스는 java.lang.Object 클래스로, Object 클래스의 메소드를 통해 클래스의 기본적인 행동을 정의할 수 있다.

```java
  public class Inheritance{}
  public class Inheritance extends Object{} //실행 결과는 같다
```

### toString()

![img](https://github.com/dilmah0203/TIL/blob/main/Image/toString().png)

- 객체를 문자열로 변환하기 위한 메소드로 오버라이딩 되어 있지 않을 경우에는 클래스이름@16진수코드 를 반환
  - System.out.println() 메소드에 매개 변수로 들어가는 경우
  - 객체에 대하여 더하기 연산

```java
import java.util.Arrays;

public class arrayTest {

    public static void main(String[] args) {
  
        int[] array = new int[] {1,2,3,4};

	System.out.println(array.toString());
	System.out.println(Arrays.toString(array));
  }
}
```

```java
> [I@43a25848] //Object.toString()메소드의 값을 출력하는 것이고, 대상 개체의 해시코드값을 출력한다
> [1, 2, 3, 4] //java.util.Araays 패키지를 이용하여 값을 문자열 형태로 리턴
```

### equals()

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/equals().png)

- 객체가 같은지 비교하는 메소드로 같으면 true, 다르면 false를 반환한다.
  - == 연산자로 primitive type은 값을 비교하고 reference type은 주소값을 비교한다. 주소값이 아닌 객체가 같은지 여부를 비교하기 위해 equals() 메소드를 오버라이딩 해야한다.

```java
public class A {

    String name;

    public A(String name) {
        this.name = name;
    }
}
```

```java
public class B {

    public static void main(String[] args) {
	
        A a = new A("Lee");
	A b = new A("Lee");

	System.out.println(a == b);
        System.out.println(a.equals(b));
   }
}
```

```java
> false //각각 생성자를 사용하여 만들었기 때문
> false //오버라이딩 하지 않았으므로 hashcode() 값을 비교
```

**equals() 재정의**

```java
public boolean equals(Object obj) {
	return this.name == ((A)obj).name;
}
```

```java
> false
> true //다른 객체라도 같은 문자열을 가지고 있다면 같다고 판단
```
