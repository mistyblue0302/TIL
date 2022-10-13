## Sticky session과 Session Clustering

일반적으로 대용량 트래픽을 **장애 없이 처리**하기 위해 여러 대의 서버에 적절히 트래픽을 분배하는 Load Balancer를 사용한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Load%20Balancer2.png)

위 상황에서 Request A,B,C,D,E,F는 Load Balancer를 통해서 각각의 서버에 나누어진다.

만약 A 서버가 로그인 요청을 처리하게 된다면 A 서버의 세션에는 `id = "martin"`이라는 값이 저장되고 로그인 성공이라는 응답을 보낸다. 동일한 클라이언트는 다음 요청으로 유저 정보를 조회하는 `GET /users/1` 요청을 보낸다. 그러나 이 요청은 Load Balancer를 통해 C 서버로 전달 된다. C 서버의 세션에는 이 사용자가 로그인 한 사용자라는 정보가 없기 때문에, 조회 실패 후 다시 로그인하라는  로그인 페이지로 리다이렉트 될 것이다. 이러한 세션 불일치 문제를 해결하기 위한 방법 중 하나는 Sticky Session 을 사용하는 것이다.

### Sticky session

### Session Clustering