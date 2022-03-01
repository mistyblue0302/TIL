# 💡 **Object클래스**

> 자바에서 모든 클래스의 부모는 java.lang.Object

<br>

```java
  public class Inheritance{}
  public class Inheritance extends Object{} //실행 결과는 같다
```

<br>

**Object클래스가 제공하는 주요 메소드**

- toString() <br>
  객체를 문자열로 변환하기 위한 메소드, 오버라이딩 되어 있지 않을 경우에는
  클래스이름@16진수코드 를 반환

```java
public class Book{
    int bookNumber;
    String bookTitle;

    Book(int bookNumber, String bookTitle){
        this.bookNumber = bookNumber;
        this.bookTitle = bookTitle;
    }

    public String toString(){
        return bookTitle + " " + bookNumber;
    }
}
```

```java
public class Tostring{
    public static void main(String[] args){
        Book book = new Book(3, "소설");

        System.out.println(book);
        System.out.println(book.toString());
    }
}
```

```java
> 소설 3
> 소설 3
```


<br>

```java
import java.util.Arrays;

public class arraytest {

  public static void main(String[] args) {
	int[] array = new int[] {1,2,3,4};

	System.out.println(array.toString());
	System.out.println(Arrays.toString(array));
  }

}

```

```java
> [I@43a25848] //Object.toString()메소드의 값을 출력하는 것이고, 대상 개체의 해쉬코드값을 출력한다
> [1, 2, 3, 4] //java.util.Araays 패키지를 이용하여 값을 문자열 형태로 리턴
```

<br>

- equals() <br>
  객체가 같은지 비교하는 메소드이다. <br>
  최상위 클래스인 Object에 포함되어 있기 때문에 재정의하여 사용이 가능하다

  == <br>
  비교 연산자이다. <br>
  기본자료형에 대해서는 값을 비교, 참조자료형에서는 주소값을 비교

```java
public class A{
    String name;

    public A(String name){
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

**equals재정의**

```java
public boolean equals(Object obj){
	return this.name == ((A)obj).name;
}
```

```java
> false
> true //다른 객체라도 같은 문자열을 가지고 있다면 같다고 판단
```

equals 메소드를 Object 타입으로 매개변수를 받는다. <br>
Object 타입은 A 클래스의 필드를 사용할 수 없기 때문에 형변환을 해준다.
