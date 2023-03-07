## Garbage Collection(GC)

Garbage Collection은 `Heap`영역에서 **동적으로 할당했던** 메모리 영역 중 더 이상 필요 없게 된 메모리를 정리하는 기능이다.

> GC는 왜 필요할까?

GC를 도입하면 수동으로 직접 메모리를 관리하던 것에서 비롯된 에러들을 방지할 수 있고 개발자의 실수로 인한 메모리 누수를 막을 수 있다.

## GC의 동작 방식

`Stop The World`는 GC를 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되며, GC가 완료된 이후에 중단했던 작업이 재개된다. 어떤 GC 알고리즘을 사용하더라도 Stop The World는 발생한다.

- **Young Generation** : 새롭게 생성된 객체 대부분이 여기에 위치한다. 생명 주기가 짧은 객체를 GC 대상으로 하는 영역으로 해당 영역에서 발생되는 GC를 **Minor GC**라고 하며 속도가 빠르다.
- **Old Generation** : Young 영역에서 살아남은 객체가 여기로 복사된다. 해당 영역에서 발생되는 GC를 **Major GC**라고 하며 속도가 느리다.

> 왜 Heap 영역은 Young 영역과 Old 영역으로 나눌까?

대부분의 객체는 수명이 짧으므로 특정 부분만 탐색함으로써 GC의 성능을 높이기 위해 나누었다.

Heap 영역은 Young 영역과 Old 영역으로 나뉘고, Young 영역은 Eden과 두 개의 Survivor Space로 나뉜다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Heap.png)

각 영역의 **처리 절차**는 다음과 같다.

- 새로 생성한 대부분의 객체는 Eden 영역에 위치한다.
- Eden영역에 객체가 차게 되면 첫번째 Minor GC가 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동된다.
- Eden영역에서 GC가 지속적으로 발생함에 따라 Survivor 영역의 객체는 계속 쌓인다. 
- 하나의 Survivor 영역이 가득차게 되면 그 중에서 살아남은 객체를 다른 Survivor영역으로 이동시킨다. 그리고 가득찼던 Survivor 영역은 아무 데이터도 없는 상태가 된다.
- 이 과정을 반복하다가 계속해서 살아남는 객체가 있다면 Old 영역으로 이동한다. 
- Old 영역이 꽉차면 Major GC를 실행하여 Mark And Sweep 알고리즘을 통해 메모리를 해제한다.

> survivor 영역은 왜 2개일까?

메모리 **외부 단편화 발생을 방지**한다. 외부 단편화란, 메모리가 할당/해제를 반복하면 메모리 공간은 있지만 작은 단위의 메모리가 부분적으로 존재해서 실제로는 할당할 수 없는 경우를 뜻한다.

`Mark And Sweep` 알고리즘은 Root Space에서부터 해당 객체에 접근 가능한지를 메모리 해제의 기준으로 삼는다. Root Space부터 순회를 통해 연결된 객체들을 찾아내고(Mark) 연결이 끊어진 객체들은 지우는 방식이다.(Sweep) Root Space부터 연결된 객체는 Reachable, 연결되지 않은 객체는 Unreachable라고 부른다. 아래는 Sweep이후 분산되어 있던 메모리가 정리된 것을 볼 수 있다.(Compaction)

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Mark_Sweep.png)

- Mark : 사용중인 메모리와 사용하지 않는 메모리를 식별한다.
- Sweep : Mark 단계에서 사용하지 않는다고 식별된 메모리를 해제한다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/Root%20Space.png)

Root Space는 Heap 영역 메모리에 대해 참조하고 있는 영역으로 Stack의 로컬 변수, Method Area에 저장된 static 변수, Native Method Stack의 C/C++로 작성된 JNI참조가 해당된다. 

## GC의 종류

### Serial GC

Mark And Sweep 이후 메모리 파편화를 막는 Compaction 과정이 진행된다. 하나의 쓰레드로 GC를 수행하기 때문에 Stop the World의 시간이 길다. 싱글 쓰레드 환경 및 적은 메모리일 경우에 적합한 방식이다.

### Parallel GC

Serial GC와 Parallel GC를 비교한 그림이다.

![img4](https://github.com/dilmah0203/TIL/blob/main/Image/Parallel%20GC.png)

GC의 기본적인 처리과정은 Serial GC와 동일하다. 하지만 Serial GC는 GC를 처리하는 쓰레드가 하나인 것에 비해, Parallel GC는 GC를 처리하는 쓰레드가 여러 개이다. 그렇기 때문에 Serial GC보다 빠르게 객체를 처리할 수 있다. Java 8의 default GC 방식이다.

### CMS GC

다음 그림은 Serial GC와 CMS GC의 절차를 비교한 그림이다. 

![img5](https://github.com/dilmah0203/TIL/blob/main/Image/CMS%20GC.png)

초기 Mark 단계에서는 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체를 찾는 것으로 끝낸다. 따라서 Stop the World 시간은 매우 짧다. 그리고 Concurrent Mark 단계에서는 방금 살아있다고 확인한 객체 중 참조하고 있는 객체들을 따라가면서 확인한다. 이 단계의 특징은 다른 쓰레드가 실행 중인 상태에서 동시에 진행된다는 것이다.

그 다음 Remark 단계에서는 Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다. 마지막으로 Concurrent Sweep 단계에서는 메모리를 해제하는 작업을 실행한다. 이 작업도 다른 쓰레드가 실행되고 있는 상황에서 진행한다.

이러한 단계로 진행되기 때문에 Stop the World 시간이 매우 짧다. 그런데 CMS GC는 다음과 같은 단점이 존재한다.

- 다른 GC보다 메모리와 CPU를 더 많이 사용한다.
- Mark and Sweep 과정 이후 Memory 파편화를 막는 Compaction이 제공되지 않는다.

G1 GC 등장에 따라 Deprecated되었다.

### G1 GC

![img6](https://github.com/dilmah0203/TIL/blob/main/Image/G1GC_Heap.PNG)

Garbage First(G1)의 줄임말로 Heap을 일정 크기의 영역으로 나누어 객체를 할당하고 GC를 실행한다. 그러다가 해당 영역이 꽉 차면 다른 영역에 객체를 할당하고 GC를 실행한다. 즉 위에서 설명한 Young 영역에서 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식이다. Java 9 이상 부터 default GC 방식이다.

기존에 Young 영역과 Old 영역을 통째로 Compaction 하던 것과 달리 해당 영역의 일부에 대해서만 Compaction을 하면 되기 때문에 처리 속도가 더 빠르다. 또한 Garbage로 영역만을 빠르게 회수하여 빈 공간을 확보하므로 GC 빈도가 줄어든다.

단, Minor GC와 Major GC를 수행하고 나서도 공간 부족 상태가 생길 수 있다. 이 때는 Full GC가 발생하는데 이 GC는 싱글 쓰레드로 동작한다.

<br>

참고

https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html

https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/generations.html#sthref16%5D

https://d2.naver.com/helloworld/1329

[우아한Tech GC](https://www.youtube.com/watch?v=FMUpVA0Vvjw)
