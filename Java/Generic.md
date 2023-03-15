## 제네릭(Generic)

제네릭은 Java 5에 등장하였는데, 제네릭이 존재하기 전 컬렉션의 요소는 다음과 같이 출력하였다.

```java
void printCollection(Collection c) {
    Iterator i = c.iterator();
    for (k = 0; k < c.size(); k++) {
        System.out.println(i.next());
    }
}
```

컬렉션이 갖는 요소들의 합을 구하는 메소드를 구현했다고 하자.

```java
int sum(Collection c) {
    int sum = 0;
    Iterator i = c.iterator();
    for (k = 0; k < c.size(); k++) {
        sum += Integer.parseInt(i.next());
    }
    return sum;
}
```

문제는 위 메소드가 String과 같은 다른 타입을 갖는 컬렉션도 호출이 가능하다는 점이다. String 타입을 갖는 컬렉션은 컴파일 시점엔 문제가 없다가 **런타임 시점에 메소드를 호출하면** 에러가 발생한다. 그래서 타입을 지정하여 컴파일 시점에 안정성을 보장받을 수 있는 방법을 고안하였고, 제네릭이 등장하였다.

제네릭은 아래 타입들이 많이 쓰인다.

|타입|설명|
|------|------|
| T |Type|
| E |Element|
| K |Key|
| V |Value|
| N |Number|

```java
public class ClassName <T> { ... }
public Interface InterfaceName <T> { ... }
public class HashMap <K, V> { ... }
```

제네릭 타입의 클래스나 인터페이스의 경우 위와 같이 선언한다. T 타입은 해당 블록 안에서까지 유효하다. 객체 생성시에 구체적인 타입을 명시해준다.

```java
public class ClassName <T, K> { ... }
 
public class Main {
	public static void main(String[] args) {
		ClassName<String, Integer> a = new ClassName<String, Integer>();
	}
}
```

타입 파라미터로 명시할 수 있는 것은 **참조 타입**(Reference Type)밖에 올 수 없다. 즉 int와 double, char 같은 primitive type은 올 수 없다. 사용자가 정의한 클래스 타입도 가능하다.

이렇듯 **제네릭(Generic)은 클래스 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되는 것**을 의미한다. **특정(Specific) 타입을 미리 지정해주는 것이 아닌 필요에 의해 지정할 수 있도록 하는 일반(Generic) 타입이다.**
    
### 제네릭은 왜 사용할까?

### 1.컴파일 타임에 강력한 타입 검사

- 제네릭 미사용

```java
List stringList = new ArrayList<>();
stringList.add("juseon");
stringList.add(3);
//String 타입으로 캐스팅 할 경우 문제 발생, Runtime Error
String result = (String) stringList.get(0) + (String) stringList.get(1); 
```

- 제네릭 사용

```java
List<String> stringList = new ArrayList<>();
stringList.add("juseon");
stringList.add(3); //Compile Error
```

제네릭을 사용한 경우 List의 타입 매개변수를 String으로 지정하면, String 타입으로 넣어야 한다는 것을 컴파일러가 알게 된다. 따라서 `stringList.add(3);`은 Compile Error가 발생하게 된다. 제네릭을 사용하면 의도한 대로 동작한다는 것을 보장받을 수 있다.

### 2. 캐스팅(타입 변환) 제거

- 제네릭 미사용

```java
List stringList = new ArrayList<>();
stringList.add("juseon");
String result = (String) stringList.get(0);
```

- 제네릭 사용

```java
List<String> stringList = new ArrayList<>();
stringList.add("juseon");
String result = stringList.get(0);
```

제네릭을 사용하지 않을 경우, List에서 값을 꺼낼 때 매번 타입 변환을 해야하는 반면 제네릭을 사용하면 List에 저장되는 요소를 String 타입으로 제한했기 때문에 캐스팅이 필요하지 않다.

> `List<Object> objectList = new ArrayList<Integer>();`은 가능할까?

### 배열 vs 제네릭 타입

- 배열

```java
Object[] objectArray = new Integer[3];
```

배열은 Integer가 Object의 하위 타입이면 Integer 배열도 Object 배열의 하위 타입이 성립된다. 이러한 특징을 **공변**이라 하고 공변이란 타입 B가 타입 A의 하위 타입일 때 B[]는 A[]의 하위 타입이 된다. 즉, 함께 변한다.

- 제네릭 타입

```java
List<Object> objectList = new ArrayList<Integer>(); //Compile error
```

제네릭에서는 Integer가 Object의 하위 타입이더라도 컴파일 오류를 낸다. 이와 같은 특징을 **무공변**이라 하고 서로의 하위 타입도 상위 타입도 될 수 없다.

### 제네릭 타입제한의 필요성

제네릭이 등장하면서 컬렉션의 타입을 다음과 같이 지정할 수 있게 되었다.

```java
void sum(Collection<Integer> c) {
    int sum = 0;
    for (Integer e : c) {
        sum += e;
    }
    return sum;
}
```

Integer가 아닌 다른 타입을 갖는 컬렉션이 위 메소드를 호출하면 컴파일 에러를 통해 타입 안정성을 보장받을 수 있게 되었다. 하지만 제네릭은 무공변이기 때문에, 타입을 Integer에서 Object로 변경하여도 모든 타입에서 공통적으로 사용하는 메소드를 만들 방법이 없다. 이를 해결하기 위해 와일드 카드가 추가되었다.

### 와일드 카드

와일드카드는 정해지지 않은 **unknown type**이기 때문에 모든 타입에 대해 호출이 가능하다. 제네릭에서 와일드 카드의 형태는 총 세가지가 있다.

1. <?> Unbounded Wildcards : 모든 타입이 가능

2. <? extends Noodle> Upper Bounded Wildcards : Noodle과 Noodle의 하위 타입

3. <? super Noodle> Lower Bounded Wildcards : Noodle과 Noodle의 상위 타입

```java
public class Category<T> {

    public T t;

    public void set(T t) {
        this. t = t;
    }

    public T get() {
        return t;
    }
}
```

```java
public class CategoryHelper {

    public void popNoodle(Category<? extends Noodle> category) {
        Noodle noodle = category.get();

        category.set(new Noodle()); //컴파일 오휴
    }

    public void pushNoodle(Category<? super Noodle> category) {
        category.set(new Noodle());

        Noodle noodle = category.get();
    }
}
```

popNoodle() 메소드는 Category 타입 매개변수를 Noodle 타입으로 제한을 하였는데, 이렇게 하게 되면 최상위 타입인 Noodle만 들어오기 때문에 해당 타입을 안전하게 가지고 올 수 있다. 하지만 메소드에서 저장을 하려고 할 경우 Category의 매개변수화 된 타입이 Pasta라면, set 메소드에 상위타입인 Noodle이 들어올지 Pasta가 들어올지 알 수 없다. 즉 하위타입에 상위 타입을 대입할 위험이 있기 때문에 컴파일 오류가 뜨게 된다.

pushNoodle() 메소드는 Category 타입 매개변수를 Noodle 타입으로 제한을 하였기 때문에 Noodle이거나 Noodle의 상위 타입만 올 수 있다. 하지만 메소드에서 Category를 꺼내 Noodle로 변환할 경우 Noodle보다 상위 타입이 꺼내질 수도 있기 때문에 컴파일 오류가 나게 된다.

> 언제 무엇을 써야할까?

**PECS(Producer-Extends, Consumer-Super) 공식**

생성을 하는 곳에서는 extends를 쓰고 소비를 하는 곳은 super를 사용해야 한다.

```java
public class NoodleCategory<E> {

    private List<E> list = new ArrayList<>();

    public void pushAll(Collection<? extends E> box) {
        for(E e: box) {
            list.add(e);
        }
    }
}
```

pushAll() 메소드는 상한 제한을 하였기 때문에 타입을 안전하게 가지고 올 수 있다. 따라서 타입을 가지고 와서 인스턴스 list에 넣어 원소 생성이 가능하다. 이렇게 무언가를 생성하는 경우 extends를 사용하는 것이 적절하다.

```java
public class NoodleCategory<E> {

    private List<E> list = new ArrayList<>();

    public void popAll(Commection<? super E> box) {
        box.addAll(list);
        list.clear();
    }
}
```

반면 super는 하한 경계이기 때문에 안전하게 값을 저장할 수 있다. 즉 인스턴스 list의 원소를 소비해서 box에 원소를 추가하고 있다.

<br>

참고

[https://docs.oracle.com/javase/tutorial/extra/generics/wildcards.html](https://docs.oracle.com/javase/tutorial/extra/generics/wildcards.html)

[우아한Tech 제네릭](https://www.youtube.com/watch?v=w5AKXDBW1gQ&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=57&t=478s)
