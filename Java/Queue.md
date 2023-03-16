## Queue

Queue는 선형 자료구조로 주로 순서가 있는 데이터를 기반으로 선입선출을 위해 만들어진 인터페이스다. 

```java
Queue<String> queue = new LinkedList<>();
queue.add("hello world");

String s = queue.peek(); //삭제하지않고 요소를 반환
String s2 = queue.poll(); //삭제하면서 반환
```

Queue는 한쪽 방향으로만(단방향) 삽입 삭제가 가능한 반면, Queue를 상속하고 있는 Deque는 Double ended Queue라는 의미로 양쪽에서 삽입삭제가 가능한 자료구조이다.

Queue/Deque를 구현하는 클래스는 다음과 같다.

- LinkedList
- ArrayDeque
- PriorityQueue

### ArrayDeque

### PriorityQueue

우선순위 
   
