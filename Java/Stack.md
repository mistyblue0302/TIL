# 💡 **Stack**

LIFO 기능을 구현한다. Stack 클래스는 자바에서 상속을 잘못 받은 클래스다. 원래의 취지인 LIFO를 생각한다면 Vector에 속해서는 안되지만 자바의 하위 호환성을 위해 상속관계를 유지하고 있다.

peek()메소드는 데이터를 리턴만 하지만, pop은 지우고 리턴한다는 차이점이 있다.

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

**Stack클래스의 단점**

1. synchronized 키워드가 붙어 있어서 Thread safe 하지만, 멀티스레드 환경이 아닐 때 사용하면 lock이라는 작업 때문에 많은 오버헤드가 발생하게 된다.

2. Stack은 LIFO 구조를 유지하는 것이 아닌 Vector 클래스를 상속받음 으로써 중간에서 데이터를 삭제, 삽입하는 것이 가능하다. ArrayDeque는 Thread safe 하지 않다는 단점이 있지만 LIFO 구조에 적합하다.