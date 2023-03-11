## Collection

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Collection.png)

자바에서 컬렉션 프레임워크는 다수의 데이터를 처리할 수 있는 표준화된 방법을 제공하는 클래스를 통칭한다. List와 Set, Queue는 Collection 인터페이스를 구현하고, Collection 인터페이스는 다음과 같이 선언되어 있다.

```java
public interface Collection<E> extends Iterable<E>
```

Iterable 인터페이스에 선언되어 있는 메소드는 iterator() 하나로, for-each문을 통해 Collection에 저장된 요소를 순차적으로 가져온다.

