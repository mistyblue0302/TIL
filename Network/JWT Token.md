## JWT(JSON Web Token)란?

- 클라이언트와 서버 사이에서 통신할 때 권한을 위해 사용하는 **암호화된 토큰**
- 인증이 필요할 때마다 서버에 토큰과 함께 요청을 보내게 된다. 서버는 저장된 데이터가 아닌 토큰 해독을 통해 사용자를 식별할 수 있고 이를 바탕으로 인증이 진행된다.

## JWT의 구성요소

JWT는 헤더(Header), 페이로드(Payload), 서명(Signature)으로 구성되어 있다. Json(Key/Value) 형태로 정보를 넣을 수 있다. 

### 헤더(Header)

토큰의 헤더는 Token 유형, 서명 알고리즘(HS256 or RSA 등)이 담긴다.

- alg는 어떤 알고리즘으로 암호화 할 것인지 ex)HS256 or RSA
- typ는 토큰의 타입에 대한 정보가 들어있다. ex)JWT

### 페이로드(Payload)

토큰에서 사용할 정보인 클레임(Claim)이 포함되는 영역으로 전송하고자 하는 여러 데이터가 담긴다.(Claim 유형: Registered / Public / Private) 

- 등록된 클레임(Registered Claim) : 토큰 정보를 표현하기 위해 미리 정해진 종류의 데이터들로, Json(Key/Value) 형태로 다수의 정보를 넣을 수 있다.
  - iss: 토큰 발급자
  - exp: 토큰 만료 시간
  - sub: 토큰 제목
  - aud: 토큰 대상자
- 공개 클레임(Public Claim) : 사용자 정의 클레임으로, 공개용 정보를 위해 사용된다. 충돌 방지를 위해 URI 포맷을 이용한다.
- 비공개 클레임(Private Claim) : 사용자 정의 클레임으로, 서버와 클라이언트 사이에 임의로 지정한 정보를 저장한다. 

### 서명(Signature)

Base64로 인코딩 된 Header, Payload와 서버만이 가지고 있는 비밀 키를 설정한 알고리즘으로 암호화한 값이 담긴다. 생성된 토큰은 HTTP 통신을 할 때 Authorization이라는 key의 value로 사용된다. 일반적으로 value에는 Bearer이 앞에 붙여진다.

```http
{ 
    "Authorization": "Bearer {생성된 토큰 값}"
 }
```

```
// Header, Payload, Signature는 "."로 구분된다.
// 예시
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9  // Header 영역
.eyJzdWIiOiIxMjM0NTY3ODkwIiwiaWQiOiJlZ2c1MjgifQ // Payload 영역
.dxx7fHi3twlN_GnfG3kZHxkLElTi9y99n5DEby-a_TE  // Signature 영역
```

![img](https://github.com/dilmah0203/TIL/blob/main/Image/JWT%20Token.PNG)

우선 Header 데이터를 Base64로 인코딩한 결과가 첫 번째 영역(Encoded의 빨간 영역)이다. 마찬가지로 Payload 데이터를 Base64로 인코딩하여 두 번째 영역(Encoded 보라색 영역)이 채워진다. 마지막으로 파란색 Signature 영역은 Base64로 인코딩된 Header, Payload 값을 서버가 설정한 알고리즘으로 암호화한 값으로 채워진다. (HS256 방식은 서버만 알고 있는 SecretKey를 활용, RSA 방식은 서버만 가지고 있는 개인키/공개키를 활용)

## JWT의 동작원리

JWT에서는 기본적으로 공개키 암호화 방식을 사용하는데, 비대칭 암호 방식을 이용해 공개키와 비밀키를 생성하여 나누어 가지고 통신한다. 서명은 비밀키가 있는 쪽에서만 할 수 있고 공개키를 가진 누구나 이 데이터의 서명을 검증할 수 있다.

공개키를 가진 누구나 데이터를 암호화해서 데이터를 보낼 수 있지만, 비밀키를 가진 곳에서만 데이터를 복호화하여 내용을 확인할 수 있다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/JWT%20Token2.png)

1. 클라이언트가 id와 password를 입력하고 서버로 로그인 요청을 보낸다.
2. 서버는 비밀키를 통해 서명을하고 공개키로 암호화시킨 JWT 토큰을 발급한다.
3. JWT 토큰을 클라이언트에게 보낸다. (로그인 완료 상태)
4. 클라이언트에서 서버로 서비스 요청 시 Authorization Header에 JWT Token을 같이 보낸다.
5. 서버에서 클라이언트가 보낸 JWT Token의 서명을 비밀키로 복호화하여 유효한 토큰인지 확인한다.
6. 서버가 요청에 대한 응답을 클라이언트에게 전달한다.

사용자 인증에 필요한 별도의 저장소가 필요없다는 장점이 있지만, 토큰은 상태를 저장하지 않으므로 임의로 삭제하는 것이 불가능하며 토큰 자체에 정보를 담고 있어 정보 노출 가능성이 있다.

<br>

참고

https://jwt.io/introduction
