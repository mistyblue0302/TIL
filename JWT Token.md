## JWT란?

**JWT**는 **Json Web Token**의 약자로 클라이언트와 서버 사이에서 통신할 때 권한을 위해 사용하는 토큰이다. 웹 상에서 정보를 Json 형태로 주고 받기 위해 표준규약에 따라 생성한 **암호화된 토큰**으로 복잡하고 읽을 수 없는 string 형태로 저장되어있다.

### JWT의 구성요소

JWT는 헤더(Header), 페이로드(Payload), 서명(Signature)으로 구성되어 있다. Json(Key/Value) 형태로 정보를 넣을 수 있다. 

![img](https://github.com/dilmah0203/TIL/blob/main/Image/JWT%20Token.PNG)

1. **헤더(Header)**

토큰의 헤더는 alg와 typ 두 가지 정보로 구성된다.

- alg는 어떤 알고리즘으로 암호화 할 것인지 ex)HS256 or RSA
- typ는 토큰의 타입에 대한 정보가 들어있다. ex)JWT


2. **페이로드(Payload)**

토큰에서 사용할 정보인 클레임(Claim)이 담겨 있다. 

- 등록된 클레임(Registered Claim)은 토큰 정보를 표현하기 위해 미리 정해진 종류의 데이터들로, Json(Key/Value) 형태로 다수의 정보를 넣을 수 있다.
    - iss: 토큰 발급자
    - exp: 토큰 만료 시간
    - sub: 토큰 제목
    - aud: 토큰 대상자

- 공개 클레임(Public Claim) 

공개 클레임은 사용자 정의 클레임으로, 공개용 정보를 위해 사용된다. 충돌 방지를 위해 URI 포맷을 이용한다.

- 비공개 클레임(Private Claim) 

비공개 클레임은 사용자 정의 클레임으로, 서버와 클라이언트 사이에 임의로 지정한 정보를 저장한다. 

3. **서명(Signature)**

서명은 토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드이다. 서명에서 사용하는 알고리즘은 헤더에서 정의한 alg를 활용한다. 위에서 만든 헤더와 페이로드의 값을 각각 BASE64Url로 인코딩하고, 인코딩한 값을 비밀 키를 이용해 헤더에서 정의한 알고리즘으로 해싱하고, 다시 BASE64Url로 인코딩하여 생성한다.

생성된 토큰은 HTTP 통신을 할 때 Authorization이라는 key의 value로 사용된다. 일반적으로 value에는 Bearer이 앞에 붙여진다.

```http
{ 
    "Authorization": "Bearer {생성된 토큰 값}"
 }
```

### JWT의 동작원리

<br>

참고

https://jwt.io/introduction
