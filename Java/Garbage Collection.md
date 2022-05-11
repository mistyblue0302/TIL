# 💡 **Garbage Collection(GC)**

> Java로 작성된 프로그램은 JVM에서 실행 되는데, 이 때 사용되는 객체를 자동으로 관리해 주는 것을 GC라고 한다. 

<br>

**Young Generations**

생명 주기가 짧은 '젊은 객체'를 GC 대상으로 하는 영역으로, Eden과 두 개의 Survivor Space로 구성 되어 있다. 해당 영역에서 발생되는 GC를 **Minor GC**라고 하며 속도가 빠르다.

**Old Generations**

생명 주기가 긴 '오래된 객체'를 GC 대상으로 하는 영역으로, 해당 영역에서 발생되는 GC를 **Major GC**라고 하며 속도가 느리다.

<br>

Old 영역의 객체에서 Young 객체로의 참조는 어떻게 이루어질까? 이러한 경우를 위해 Card Table이 존재한다.

![img](https://d2.naver.com/content/images/2015/06/helloworld-1329-2.png)

Card Table에는 Old 객체가 Young 객체를 참조할 때마다 정보가 표시된다. Young 영역의 GC를 수행할 때에는 Old 영역의 모든 객체를 확인하지 않고, Card Table만 조회하여 GC의 대상인지 판별한다. 만약 모든 Old 영역에 존재하는 객체를 확인한다면 비효율적이기 때문이다.

<br>

**Young 영역의 구성**

Young 영역은 Eden, Survivor이라는 2개의 영역으로 나뉜다.

- 최초에 객체 생성 시 Eden 영역에 생성된다.
- Eden영역에 객체가 차게 되면 첫번째 GC가 일어난다.
- 살아남은 객체는 Survivor 영역 중 하나로 이동한다.
- Eden영역에서 GC가 지속적으로 발생함에 따라 Survivor 영역으로 객체는 계속 쌓인다.
- 하나의 Survivor 영역이 가득하게 되면 살아남은 객체를 또 다른 Survivor영역으로 이동시킨다. 그리고 가득찼던 Survivor 영역은 아무 데이터가 없는 상태가 된다.
- 이 과정을 반복하다가 계속해서 살아남는 객체가 있다면 Old 영역으로 이동한다.

**survivor 영역은 왜 2개일까?**

메모리 **외부 단편화 발생을 방지**한다. 외부 단편화란, 메모리가 할당/해제를 반복하면 메모리 공간은 있지만 작은 단위의 메모리가 부분적으로 존재해서 실제로는 사용할 수 없는 경우를 말한다.

<br>

## **GC의 동작 방식**

### 1. **Stop The World**

  GC를 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되며, GC가 완료되면 작업이 재개된다.


### 2. **Mark and Sweep**

- Mark : 사용중인 메모리와 사용하지 않는 메모리를 식별한다

- Sweep : Mark 단계에서 사용하지 않는다고 식별된 메모리를 해제한다

  Stop the World 과정을 거쳐 모든 작업을 중단하게 되면, GC는 어떤 객체를 참조하고 있는지 탐색하게 된다. 사용중인 메모리를 식별하면(Mark) 이후에 사용하지 않는 객체를 메모리에서 제거(Sweep)한다.

<br>

## **GC의 종류**

### **Parallel GC**

  멀티 스레드를 사용하여 GC를 수행한다. 그렇기 때문에 빠르게 객체를 처리할 수 있다.

### **G1 GC**

  G1 GC는 Heap을 Young, Old 영역으로 명확하게 구분짓던 GC와는 달리 물리적으로 구분을 하지 않는다. 고정된 크기가 없기 때문에 메모리 사용에 있어 유연성을 제공한다. 다음 그림처럼 각 영역에 객체를 할당하고 GC를 실행한다. 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행하게 된다.


![img2](https://github.com/dilmah0203/TIL/blob/main/Image/G1GC.png)


<br>

참고

https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html

https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/generations.html#sthref16%5D

https://d2.naver.com/helloworld/1329
