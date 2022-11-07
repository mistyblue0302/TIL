## 쓰레드 풀(Threadpool)이란?

병렬 작업을 처리하기 위해 멀티 쓰레드를 사용하게 되는데, 쓰레드가 과도하게 증가할 경우 **쓰레드 생성과 Context-Switching**으로 인해 메모리 문제와 CPU 오버헤드가 발생할 수 있다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Thread%20Pool.PNG)

이러한 단점 때문에, **쓰레드 풀**을 사용한다. 쓰레드 풀은 쓰레드를 제한된 개수만큼 정해놓고 작업 큐(Queue)에 들어오는 작업들을 쓰레드가 맡아 처리한다. 작업이 끝난 쓰레드는 없어지지 않고 다시 작업 큐에서 새로운 작업을 가져와 처리한다. 쓰레드 풀은 쓰레드를 재사용하기 때문에 새로운 쓰레드를 생성하지 않아도 되고, 쓰레드 개수가 제한되어 있어서 오버헤드 가능성이 적다.

## Java의 Thread Pool

Java는 `ThreadPoolExecutor` 클래스로 Thread Pool을 구현하고 있다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/ThreadPoolExecutor.png)

**maximumPoolSize**는 쓰레드 풀 안에서 만들 수 있는 최대 쓰레드 개수를 의미하는데, 항상 최대 개수를 유지하지 않는다. 이유는 요청이 적을 경우 많은 양의 쓰레드가 필요하지 않기 때문이다. **corePoolSize**는 최소 쓰레드 개수로 항상 유지한다. **keepAliveTime**은 corePoolSize보다 쓰레드가 많아졌을 경우, 초과한 쓰레드에 대해서 keepAliveTime 값보다 오랜 시간동안 작업을 할당받지 못할 경우 제거된다. 이 값은 자원의 낭비를 감소시켜 효율적으로 쓰레드 풀을 관리할 수 있게된다.

## Tomcat의 Thread Pool

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Thread%20Pool2.png)

Tomcat은 Java의 Thread Pool과 유사한 자체 쓰레드 풀 구현체를 가지고 있다. 웹 요청이 들어오면 Tomcat의 Connector가 Connection을 생성하면서 요청된 작업을 Thread Pool의 쓰레드에 연결한다. 이 때 Tomcat이 동시에 최대로 처리할 수 있는 Connection의 개수가 **maxConnections**다. **acceptCount**는 maxConnections 이상의 요청이 들어왔을 때 사용하는 대기열 큐 사이즈로 추가적으로 들어오는 요청은 거절될 수 있다.

## 어떻게 Thread Pool을 설정해야할까?

- 쓰레드 개수를 요청 수에 비해 너무 많게 설정하면, 작업 중이지 않은 쓰레드가 많아져서 메모리 낭비가 생길 수 있다.

- 쓰레드 개수를 요청 수에 비해 적게 설정하면, 동시에 처리할 수 있는 작업이 적어지기 때문에 평균 응답시간이 감소한다.

<br>

참고

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadPoolExecutor.html

[Thread Pool](https://www.youtube.com/watch?v=um4rYmQIeRE&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=3)
