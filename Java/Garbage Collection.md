## Garbage Collection(GC)

GC는 메모리 관리 기법 중 하나로, 프로그램이 **동적으로 할당**했던 메모리 영역 중 필요 없게 된 영역을 알아서 해제해준다. 여기서 동적으로 할당된 메모리란, 프로그램 런타임에 사용되는 **Heap 영역** 메모리를 뜻하고 필요 없게 된 영역은 어떤 변수도 가리키지 않게 된 영역을 의미한다.

> GC는 왜 필요할까?

GC를 도입하면 수동으로 메모리를 관리하던 것에서 비롯된 에러들을 방지할 수 있고 개발자의 실수로 인한 메모리 누수를 막을 수 있다.

**Young Generations**

생명 주기가 짧은 '젊은 객체'를 GC 대상으로 하는 영역으로, Eden과 두 개의 Survivor Space로 구성 되어 있다. 해당 영역에서 발생되는 GC를 **Minor GC**라고 하며 속도가 빠르다.

**Old Generations**

생명 주기가 긴 '오래된 객체'를 GC 대상으로 하는 영역으로, 해당 영역에서 발생되는 GC를 **Major GC**라고 하며 속도가 느리다.

Old 영역의 객체에서 Young 객체로의 참조는 어떻게 이루어질까? 이러한 경우를 위해 Card Table이 존재한다.

![img](https://d2.naver.com/content/images/2015/06/helloworld-1329-2.png)

Card Table에는 Old 객체가 Young 객체를 참조할 때마다 정보가 표시된다. Young 영역의 GC를 수행할 때에는 Old 영역의 모든 객체를 확인하지 않고, Card Table만 조회하여 GC의 대상인지 판별한다. 만약 모든 Old 영역에 존재하는 객체를 확인한다면 비효율적이기 때문이다.

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

### GC의 동작 방식

GC는 어떻게 해제할 동적 메모리 영역들을 알아서 판단할까?

**1. Stop The World**

GC를 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되며, GC가 완료되면 작업이 재개된다.
  
**2.Reference Counting**

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Reference%20Counting.png)

Root Space는 스택 변수, 전역 변수 등 Heap 영역 참조를 담은 변수이다. Reference Counting은 Heap 영역에 선언된 객체들이 각각 reference count를 가지고 있는데 여기서 reference count는 몇 가지 방법으로 해당 객체에 접근할 수 있는지를 뜻한다. 해당 객체에 접근할 수 있는 방법이 없다면 즉, reference count가 0이면 GC의 대상이 된다. 하지만 Reference Counting은 순환 참조 문제가 있다. 두개 이상의 객체가 서로 참조를 하고있다면, 순환참조가 생겨서 참조 횟수가 영원히 0이 될 수 없기때문에 Memory Leak의 원인이 된다.

**3. Mark and Sweep**

Mark and Sweep 알고리즘은 Reference Counting의 순환 참조 문제를 해결할 수 있다. Root에서부터 해당 객체에 접근 가능한지를 해제의 기준으로 삼는다. Root부터 그래프 순회를 통해 연결된 객체들을 찾아내고(Mark) 연결이 끊어진 객체들은 지우는 방식이다.(Sweep)

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/Mark_Sweep.png)

- Mark : 사용중인 메모리와 사용하지 않는 메모리를 식별한다
- Sweep : Mark 단계에서 사용하지 않는다고 식별된 메모리를 해제한다
- Root로부터 연결된 객체를 reachable, 연결되지 않았다면 unreachable 

위 그림에선 Sweep이후 분산되어 있던 메모리가 정리된 것을 볼 수 있는데, 이를 Memory 파편화를 막는 Compaction이라고 한다. Mark and Sweep 알고리즘을 사용하면 Root로부터 연결이 끊긴 순환 참조되는 객체들도 모두 지울 수 있다.

### GC의 종류

- **Parallel GC**

  멀티 스레드를 사용하여 GC를 수행한다. 그렇기 때문에 빠르게 객체를 처리할 수 있다.

- **G1 GC**

  G1 GC는 Heap을 Young, Old 영역으로 명확하게 구분짓던 GC와는 달리 물리적으로 구분을 하지 않는다. 고정된 크기가 없기 때문에 메모리 사용에 있어 유연성을 제공한다. 다음 그림처럼 각 영역에 객체를 할당하고 GC를 실행한다. 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행하게 된다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/G1GC.png)

참고

https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html

https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/generations.html#sthref16%5D

https://d2.naver.com/helloworld/1329
