## List

**배열**은 데이터를 연속된 공간에 저장하는 구조로, 다음과 같은 단점이 있다.

- 자료형이 같아야한다.
- 크기가 고정되어 있다.

**List**는 배열처럼 객체를 일렬로 늘어놓은 구조로 **순서**가 있으며 **중복을 허용**한다. 객체를 **인덱스**로 관리하기 때문에 List에 객체를 추가하면 자동으로 인덱스가 부여된다. 인덱스를 참조하여 객체를 찾고, 만약 동일한 객체가 저장되어 있다면 동일한 인덱스를 참조한다. List는 객체를 추가(add), 검색(contains, get, size), 삭제(remove, clear)하는 기능의 메소드가 선언되어 있다. 

```java
List<String> list = new ArrayList<>();
list.add("hello world");
list.add(null);

boolean value = list.contains("hello world");
int size = list.size();

list.remove(0);

for (String s : list) {
    System.out.println(s);
}
```

List 인터페이스를 구현한 클래스는 다음과 같다.

- ArrayList
- Vector
- Stack
- LinkedList

### ArrayList

ArrayList는 List처럼 인덱스로 객체를 관리하지만 크기를 동적으로 늘릴 수 있다는 점에서 일반 배열과 차이가 있다. 또한 Thread safe하지 않다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/ArrayList.png)

ArrayList의 초기 저장 용량은 10이다. 처음 저장 용량 크기를 넘게 되면 자동적으로 저장 용량이 늘어난다.

```java
List<String> list = new ArrayList<>(); //기본 저장 용량 10
List<String> list = new ArrayList<>(100); //저장 용량을 100으로 설정
```

ArrayList는 배열과 유사하지만 특정 인덱스의 객체를 제거할 경우, 제거한 객체의 인덱스부터 마지막 인덱스까지 모두 앞으로 1칸씩 이동하게 된다. 객체를 추가할 경우 1칸씩 뒤로 이동하게 된다. 객체의 삭제와 삽입이 빈번하게 일어날 경우 모든 객체가 재정렬 되기 때문에 사용하지 않는 것이 좋다. ArrayList는 인덱스가 있으므로 객체의 검색이나 가장 마지막 위치에 객체 추가를 할 경우 성능이 좋다.

### Vector

Vector와 ArrayList의 차이점으로는 Vector 클래스는 synchronized 메소드로 구성되어 있다. 그렇기 때문에 멀티 쓰레드 환경에서 안전하다.(Thread safe하다) 하지만 동기화되어 있기 때문에 ArrayList보다는 객체를 추가, 삭제하는 과정이 느리다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Vector.png)

```java
List<String> list = new Vector<>();
list.add("hello world");
list.remove(0);
```

### Stack

LIFO 기능을 위한 클래스로 모든 메서드에 synchronized가 붙어있어 Thread safe하다. Stack은 모든 작업에 Lock이 사용되기 때문에 성능이 저하될 수 있다. Deque의 구현체인 ArrayDeque는 Thread safe하지 않지만 더 빠르다.

### LinkedList

ArrayList는 내부 배열에 객체를 저장하여 인덱스로 관리하는 반면 LinkedList는 인접한 참조들을 연결하여 관리한다. LinkedList에서 특정 인덱스의 객체를 제거하면, 제거되는 인덱스의 앞 뒤 연결만 변경되고 나머지는 변경되지 않는다. ArrayList가 제거되는 인덱스를 기준으로 뒤의 객체들이 한칸씩 이동했었던 점과 차이가 있다. 이러한 차이로 인해 객체를 삽입, 삭제할 때는 LinkedList가 성능이 좋다.



