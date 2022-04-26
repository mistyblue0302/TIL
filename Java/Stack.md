# 💡 **Stack**

Stack 클래스는 자바에서 상속을 잘못 받은 클래스다. 원래의 취지인 LIFO를 생각한다면 Vector에 속해서는 안되지만 자바의 하위 호환성을 위해 상속관계를 유지하고 있다. 

peek()메소드는 데이터를 리턴만 하지만, pop()은 지우고 리턴한다는 차이점이 있다.

<br>

```java
Stack<Integer> intStack = new Stack<Integer>();
  for (int loop = 0; loop < 3; loop++) {
      intStack.push(loop);
      System.out.println(intStack.peek());
  }
  System.out.println(intStack.size()); //3
```

```java
Stack<Integer> intStack = new Stack<Integer>();
  for (int loop = 0; loop < 3; loop++) {
      intStack.push(loop);
      System.out.println(intStack.pop());
  }
  System.out.println(intStack.size()); //0
```

Vector는 get()과 set()역할을 하는 모든 메서드에 synchronized 키워드가 붙어 있다. 멀티스레드 환경이 아닐 때 사용하면 lock이라는 작업 때문에 많은 오버헤드가 발생해 성능을 저하시킬 수 있다. 따라서 Vector는 특정 상황에서만 최적으로 동작하게 되고, 어떤 상황에서는 그렇지 않게 되므로 효율적인 Thread-safe 컬렉션이라고 할 수 없다.

그럼 어떤 컬렉션을 쓰는게 좋은가?

ArrayList를 사용하면된다.

```java
ArrayList<T> al = new ArrayList<>(Collections.synchronizedList());
```

이와같이 ArrayList의 생성자에 매개변수로 Collections.synchronizedList()를 넘겨주게되면 thread-safe한 ArrayList를 사용할 수 있다.

Stack이 사용되지 않아야 하는 이유도 Vector의 경우와 같다. Vector를 상속받음으로써 Stack은 Vector의 기능을 전부 사용할 수 있게 되므로 완벽한 LIFO 구조를 가질 수 없다. ArrayDeque는 Thread safe 하지 않다는 단점이 있지만 LIFO 구조에 적합하다.
