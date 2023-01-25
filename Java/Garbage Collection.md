## Garbage Collection(GC)

JVM에는 Garbage Collecter가 존재한다. Garbage Collecter는 `Heap`영역에서 **동적으로 할당했던** 메모리 영역 중 더 이상 필요 없게 된 메모리를 주기적으로 정리해주는 역할을 한다. Garbage Collecter가 주기적으로 메모리 누수 방지를 하기 위해 메모리를 정리하는 과정을 Garbage Collection이라고 한다. 

> GC는 왜 필요할까?

GC를 도입하면 수동으로 직접 메모리를 관리하던 것에서 비롯된 에러들을 방지할 수 있고 개발자의 실수로 인한 메모리 누수를 막을 수 있다.

## GC의 동작 방식

GC는 어떻게 해제할 동적 메모리 영역들을 알아서 판단할까?

`Stop The World`는 GC를 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되며, GC가 완료되면 작업이 재개된다. 어떤 GC 알고리즘을 사용하더라도 Stop The World는 발생한다.
  
- **Young Generation** : 새롭게 생성된 객체 대부분이 여기에 위치한다. 생명 주기가 짧은 객체를 GC 대상으로 하는 영역으로 해당 영역에서 발생되는 GC를 **Minor GC**라고 하며 속도가 빠르다.
- **Old Generation** : Young 영역에서 살아남은 객체가 여기로 복사된다. 해당 영역에서 발생되는 GC를 **Major GC**라고 하며 속도가 느리다.

Young 영역은 Eden과 두 개의 Survivor Space로 총 3개의 영역으로 나뉜다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Heap.png)

각 영역의 처리 절차는 다음과 같다.

- 새로 생성한 대부분의 객체는 Eden 영역에 위치한다.
- Eden영역에 객체가 차게 되면 첫번째 Minor GC가 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동된다.
- Eden영역에서 GC가 지속적으로 발생함에 따라 Survivor 영역의 객체는 계속 쌓인다. 
- 하나의 Survivor 영역이 가득차게 되면 그 중에서 살아남은 객체를 다른 Survivor영역으로 이동시킨다. 그리고 가득찼던 Survivor 영역은 아무 데이터도 없는 상태가 된다.
- 이 과정을 반복하다가 계속해서 살아남는 객체가 있다면 Old 영역으로 이동한다. 

> survivor 영역은 왜 2개일까?

메모리 **외부 단편화 발생을 방지**한다. 외부 단편화란, 메모리가 할당/해제를 반복하면 메모리 공간은 있지만 작은 단위의 메모리가 부분적으로 존재해서 실제로는 사용할 수 없는 경우를 말한다.

`Mark And Sweep` 알고리즘은 Root에서부터 해당 객체에 접근 가능한지를 해제의 기준으로 삼는다. Root Space부터 연결된 객체들을 찾아내고(Mark) 연결이 끊어진 객체들은 지우는 방식이다.(Sweep) Root Space부터 연결된 객체는 Reachable, 연결되지 않은 객체는 Unreachable라고 부른다. Sweep이후 분산되어 있던 메모리가 정리된 것을 볼 수 있다.(Compaction)

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Mark_Sweep.png)

- Mark : 사용중인 메모리와 사용하지 않는 메모리를 식별한다
- Sweep : Mark 단계에서 사용하지 않는다고 식별된 메모리를 해제한다
- Root로부터 연결된 객체를 reachable, 연결되지 않았다면 unreachable 

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/Root%20Space.png)

JVM의 GC는 기본적으로 Mark and Sweep 방식으로 동작하는데 JVM 메모리의 Stack의 로컬 변수, Method Area에 저장된 static 변수, Native Method Stack의 C/C++로 작성된 JNI참조가 Root Space가 된다. 

## GC의 종류

### Serial GC

하나의 쓰레드로 GC를 수행하기 때문에 Stop the World의 시간이 길다.
싱글 쓰레드 환경 및 Heap이 작을 때 사용

### Parallel GC
  - 멀티 스레드를 사용하여 GC를 수행하기 때문에 Stop the World의 시간이 짧다.
  - Java 8의 default GC 방식

### CMS GC
  - Stop the World 최소화를 위해 고안
  - GC 작업을 애플리케이션과 동시에 실행
  - G1 GC 등장에 따라 Deprecated
  -  Mark and Sweep 과정 이후 Memory 파편화를 막는 Compaction이 제공되지 않는 단점
 
![img6](https://github.com/dilmah0203/TIL/blob/main/Image/G1GC_Heap.PNG)

### G1 GC
  - Garbage First(G1)
  - Heap을 일정 크기의 Region으로 나누어 Young Generation, Old Generation 영역으로 활용한다.
  - 런타임에 G1 GC가 필요에 따라 영역별 Region 개수를 튜닝하여 Stop the World를 최소화
  - Java 9 이상 부터 default GC 방식
  - 고정된 크기가 없기 때문에 메모리 사용에 있어 유연성을 제공한다.

<br>

참고

https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html

https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/generations.html#sthref16%5D

https://d2.naver.com/helloworld/1329

[우아한Tech GC](https://www.youtube.com/watch?v=FMUpVA0Vvjw)
