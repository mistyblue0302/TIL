# 💡 **Garbage Collection(GC)**

> Java로 작성된 프로그램은 JVM에서 실행 되는데, 이 때 사용되는 객체를 자동으로 관리해 주는 것을 GC라고 한다. 객체는 Heap 영역에 저장되는데 Young Generation과 Old Generation으로 구성되어 있다.

- Young Generations
생명 주기가 짧은 '젊은 객체'를 GC 대상으로 하는 영역으로, Eden과 두 개의 Survivor Space로 구성 되어 있다. 해당 영역에서 발생되는 GC를 minor GC라고 하며 속도가 빠르다.

- Old Generations
생명 주기가 긴 '오래된 객체'를 GC 대상으로 하는 영역으로, 해당 영역에서 발생되는 GC를 Major GC라고 하며 속도가 느리다.



```java