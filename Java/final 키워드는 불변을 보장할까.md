# 💡 **final 키워드는 불변을 보장할까?**

> final 키워드는 해당 오브젝트가 단 한번 할당할 수 있음을 의미한다.

- 변수에 선언될 경우 수정이 불가능하며,

- 메소드에 선언되면 오버라이딩이 불가능해진다. 

- 또한 클래스에 붙이면 상속이 불가능한 클래스가 된다.

<br>

하지만 Collection의 경우는 다르다.

```java
final List<String> list = new ArrayList<>();
```

List가 final 객체일 때는 수정이 가능하다. 이유는 final List는 객체의 값을 가지고 있는 것이 아닌,
객체의 주소를 참조하고 있기 때문이다. 그렇기 때문에 주소 변경이 불가능해진다.

```java
Collections.unmodifiableList(list);
```

이처럼 unmodifiableList를 활용하면 수정이 불가능해진다. 
