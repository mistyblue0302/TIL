## Blocking vs Non-Blocking, Sync vs Async

### 1. Blocking vs Non-Blocking

다른 주체가 작업할 때 자신에게 자신의 작업에 대한 제어권이 있는지 없는지에 따라 Blocking과 Non-Blocking을 나눈다.

**Blocking(블로킹)**

- 자신의 작업을 진행하다가 다른 쓰레드의 작업이 시작되면 그 작업이 끝날 때까지 **기다렸다가** 자신의 작업을 시작하는 것을 말한다. 즉 다른 작업 동안 자신의 작업에 대한 제어권이 없다. 자바에서는 synchronized를 통해 블로킹을 지원한다.

**Non-Blocking(논블로킹)**

- 다른 쓰레드의 작업여부와 **상관없이** 자신의 작업을 할 수 있는 상태로 제어권을 위임하지 않는다. 자바는 Atomic을 통해 논블로킹을 지원한다.

### 2. Sync vs Async

결과 리턴 시 순서와 결과에 관심이 있는지 없는지로 판단할 수 있다.

**Synchronous(동기)**

- 작업을 동시에 수행하거나, 동시에 끝나거나, 끝나는 동시에 시작함을 의미
- 작업을 요구한 쪽에서 작업 수행 완료 여부에 대해 관심이 있음

**Asynchronous(비동기)**

- 시작, 종료가 일치하지 않으며 끝나는 동시에 시작하지 않음을 의미
- 작업을 요구하면서 callback 함수도 같이 전달한다. 작업 요청을 받은 쪽에서 작업이 완료되면 callback을 실행한다.
- 작업을 맡겨놓고 결과에 신경 쓰지 않음

### 3. Blocking/Non-Blocking, Sync/Async

- **Blocking/Sync**

작업을 요청하면 요청받은 쪽에게 제어권이 넘어가고, 작업이 완료되면 바로 해당 업무를 처리한다. 자바에서 입력 요청을 할 때 Blocking/Sync를 사용한다. 제어권이 넘어갔기 때문에 `출력할 메세지를 입력` 아래의 내용이 실행되지 않는다. 입력을 하면 제어권과 결과를 받아 처리하게 된다.

```java
public class Application {

    public static void main(String[] args) {
        System.out.println("출력할 메세지를 입력");
        Scanner scanner = new Scanner(System.in);
        String message = scanner.nextLine();
        System.out.println("Blocking/Sync");
        System.out.println(message);)
    }
}
```

- **NonBlocking/Sync**

작업을 요청하고 자신의 일을 하면서 주기적으로 작업을 요청한 곳에게 일을 마쳤는지 물어본다. 작업이 완료되면 해당 결과를 가지고 바로 업무를 처리한다.

- **Blocking/Async**

자신의 작업에 대한 제어권이 없고, 다른 작업의 결과를 바로 처리하지 않아도 된다.

- **NonBlocking/Async**

다른 작업이 시작되어도 자신이 하던 작업을 멈추지 않고, 다른 작업에서 끝난 결과를 바로 처리하지 않아도 된다.

<br>

참고

[우아한Tech Blocking vs Non-Blocking, Sync vs Async](https://www.youtube.com/watch?v=oEIoqGd-Sns&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=126)

[우아한Tech Block vs Non-Block & Sync vs Async](https://www.youtube.com/watch?v=IdpkfygWIMk&t=1118s)
