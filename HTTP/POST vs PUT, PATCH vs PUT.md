## POST vs PUT
 
### POST

```HTTP
POST /members HTTP/1.1
Content-Type: application/json

{
  "username": "hello",
  "age": 20
}
```

- **리소스 결정권이 클라이언트가 아닌 서버가 가지고 있다.**
  - 클라이언트는 /members/100에 만들어질지, /members/200에 만들어질지 알 수 없다.
- POST는 멱등하지 않다. 여러번의 재시도에 대한 모든 결과값이 동일하지 않다는 것이다. 즉, POST로 동일한 요청을 N번 보낸다면, N개의 다른 리소스들이 생성되는 것이다.
- POST Request의 Response는 Caching 가능 하다.

### PUT

```HTTP
PUT /members/100 HTTP/1.1
Content-Type: application/json

{
  "username": "hello",
  "age": 20
}
```

- **리소스를 대체**
  - 리소스가 있으면 덮어버린다.
  - 리소스가 없으면 생성
- **클라이언트가 리소스를 식별**
  - 클라이언트가 리소스 위치를 알고 URI를 지정한다.
- PUT는 멱등하다. PUT request로는 새로운 정보가 계속해서 생성되지 않는다. 
- POST Request의 Response는 Caching 할 수 없다.

## PATCH vs PUT

### PATCH

```HTTP
PATCH /members/100 HTTP/1.1
Content-Type: application/json

{
  "age": 10
}
```

- **리소스 부분 변경**
  - PUT의 경우 age만 변경할 경우 username 필드는 삭제된다.
  - PATCH는 username은 삭제되지 않고 age만 변경된다.

### PUT

- 리소스 변경 시 모든 필드가 필요하다. 만약 전체가 아닌 일부 필드만 전달할 경우 전달한 필드 외 모두 null값이 되거나 초기값으로 처리된다.
