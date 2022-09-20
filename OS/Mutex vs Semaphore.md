## Mutex vs Semaphore

Mutex와 Semaphore는 모두 공유자원의 **상호배제**를 달성하기 위해 사용된다. 즉, 동시성 프로그래밍에서 여러 프로세스나 쓰레드가 공유자원에 접근하는 것을 제어하기 위한 방법이다.

### Mutex

Mutex는 멀티 쓰레드 환경에서 자원에 대한 접근을 동기화하기 위해 사용된다. 동기화 대상이 하나이다.

> 동기화란? 공유 리소스에 대한 스레드 액세스를 제어하는 것

Mutex는 공유자원을 사용중인 쓰레드가 있을 때, 다른 쓰레드가 공유자원에 접근하면 Blocking 후 대기 큐로 보낸다. Boolean타입의 Lock 변수를 사용하고, Lock을 건 쓰레드만 임계 영역을 나갈 때 Lock을 해제 할 수 있다. 이유는 Mutex는 1개의 Lock을 가지기 때문이다.

### Semaphore

Mutex와 차이점은 Semaphore는 동기화 대상이 하나 이상이다. Semaphore 변수를 통해 wait, signal을 관리하며 Lock을 걸지 않은 쓰레드도 signal을 보내 Lock 해제가 가능하다.

<br>

참고

[우아한Tech Mutex vs Semaphore](https://www.youtube.com/watch?v=oazGbhBCOfU)
