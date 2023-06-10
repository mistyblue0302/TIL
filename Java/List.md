## List

**배열**은 데이터를 연속된 공간에 저장하는 구조로, 다음과 같은 단점이 있다.

- 자료형이 같아야한다.
- 크기(길이)가 고정되어 있다.
- index에 위치한 하나의 데이터를 삭제하면 해당 index는 빈공간으로 계속 남는다.

**List**는 배열처럼 데이터들을 일렬로 늘어놓은 구조로 길이가 가변적이다. 또한 **순서**가 있으며 **중복을 허용**하고, 데이터 사이에 빈 공간이 없고 연속되어 있다. 객체를 **index**로 관리하기 때문에 List에 객체를 추가하면 자동으로 index가 부여되고, index를 참조하여 객체를 찾는다. List는 객체를 추가(add), 검색(contains, get, size), 삭제(remove, clear)하는 기능의 메소드가 선언되어 있다. 

```java
List<String> list = new ArrayList<>();
list.add("hello world");
list.add(null);

String result = list.get(0);

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

ArrayList는 최상위 타입인 오브젝트 배열(Object[])을 사용하여 데이터를 담아 index로 요소를 관리하지만 **크기를 동적으로 늘릴 수 있다**는 점에서 일반 배열과 차이가 있다. 또한 Thread safe하지 않다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/ArrayList.png)

ArrayList의 초기 저장 용량은 10이다. 처음 저장 용량 크기를 넘게 되면 자동적으로 저장 용량이 늘어난다. 용량이 늘어나면 기존 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장되는데 이 과정에서 처리 시간이 많이 소요되기 때문에 초기 용량을 설정하는 것이 좋다.

```java
List<String> list = new ArrayList<>(); //기본 저장 용량 10
List<String> list = new ArrayList<>(100); //저장 용량을 100으로 설정
```

ArrayList는 배열과 유사하지만 특정 index의 요소를 제거할 경우, 제거한 요소의 index부터 마지막 index까지 모두 앞으로 1칸씩 이동하게 된다. 요소를 추가할 경우 1칸씩 뒤로 이동하게 된다. 요소의 삭제와 삽입이 빈번하게 일어날 경우 모든 객체가 재정렬 되기 때문에 성능이 좋지 않다. ArrayList는 index가 있으므로 검색이나 가장 마지막 위치에 요소를 추가할 경우 성능이 좋다.

### LinkedList

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/LinkedList.png)
![img3](https://github.com/dilmah0203/TIL/blob/main/Image/LinkedList2.png)

ArrayList는 내부 배열에 요소를 저장하여 index로 관리하는 반면 LinkedList는 인접한 **참조들을 연결하여 관리**한다. LinkedList에서 특정 요소를 제거하거나 추가하면, 앞 뒤 연결만 변경되고 나머지는 변경되지 않는다. 중간에 요소를 추가하거나 삭제하더라도 전체 index가 밀리거나 당겨지는 일이 없기 때문에 ArrayList에 비해 요소의 추가나 삭제가 용이하지만 특정 요소를 탐색할 경우 순차 탐색으로 이루어져 있어 탐색 속도가 느리다. 요소를 추가, 삭제할 때는 LinkedList가 성능이 좋다.

### Vector

Vector와 ArrayList의 차이점은 Vector 클래스는 synchronized 메소드로 구성되어 있다. 그렇기 때문에 멀티 쓰레드 환경에서 안전하다.(Thread safe하다) 하지만 동기화되어 있기 때문에 ArrayList보다는 요소를 추가, 삭제하는 과정이 느리다.

![img4](https://github.com/dilmah0203/TIL/blob/main/Image/Vector.png)

```java
List<String> list = new Vector<>();
list.add("hello world");
list.remove(0);
```

### Stack

Stack은 LIFO 기능을 위한 클래스로 모든 메서드에 synchronized가 붙어있어 Thread safe하다. 모든 작업에 Lock이 사용되기 때문에 성능이 저하될 수 있다. Deque의 구현체인 ArrayDeque는 LIFO 구조로, Thread safe하지 않지만 더 빠르다.

```java
Stack<Integer> stack = new Stack<>();
stack.push(1);
stack.push(3);
        
int value = stack.peek(); //요소를 제거하지 않고 반환
int value2 = stack.pop(); //요소를 제거하고 반환
        
int result = stack.search(3); //스택의 상단으로부터 몇 번째에 위치하는지 반환(거리)
```

