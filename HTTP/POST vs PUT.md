# 💡 **POST vs PUT**

**멱등성(Idempotence)이란?**

여러번 수행해도 결과가 같음을 의미한다 <br>

<br>

**POST**

리소스 결정권이 클라이언트가 아닌 서버가 가지고 있다

**POST의 특징**

- **POST Request는 멱등하지 않다**

  여러번의 재시도에 대한 모든 결과값이 동일하지 않다는 것이다. <br>
  즉, POST로 동일한 요청을 N번 보낸다면, N개의 다른 리소스들이 생성되는 것이다.

- **POST Request의 Response는 Caching 가능 하다**

<br>

**PUT**

리소스의 위치를 명확히 알고 요청을 할 경우 사용

- if resource 존재 -> Update(갱신) <br>
- else -> Create(생성)

**PUT의 특징**

- **PUT Request는 멱등하다**

  PUT request로는 새로운 정보가 계속되서 생성되지 않는다. <br>
  여러번 재시도를 하더라도, 동일한 결과 값을 받는다.

- **POST Request의 Response는 Caching 할 수 없다**

<br>

**그렇다면 PATCH와 PUT은?**

**PATCH**

자원의 부분적인 수정을 적용, 자원교체 시 일부 필드 필요

<br>

**PUT**

자원의 전체 교체, 자원교체 시 모든 필드 필요

만약 전체가 아닌 일부 필드만 전달할 경우 전달한 필드 외 모두 null값이 되거나 초기값으로 처리된다.
