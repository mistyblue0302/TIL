## REST API

**REST**란 서버와 클라이언트 간 통신 방식 중 하나로, **표현에 의한 상태 전달**(Representational State Transfer)이라는 뜻을 가지고 있다. 자원을 이름으로 구분하여 상태를 주고받는 것을 뜻한다.

API(Application Programming Interface)는 서로 다른 프로그램 간에 정보를 주고받을 때 사용하는 규약을 정의한 것으로, REST API란 REST 아키텍처 스타일을 따르는 API라고 정의할 수 있다. 아키텍처 스타일은 제약조건의 집합을 말한다.

## REST를 구성하는 제약조건

### 1. Client-Server(클라이언트 / 서버 구조)

Rest API에서 자원을 가지고 있는 쪽이 서버, 자원을 요청하는 쪽이 클라이언트에 해당한다. 서버와 클라이언트의 역할을 확실히 구분시킴으로써 독립적으로 진화할 수 있다.

### 2. Stateless(무상태)

서버와 클라이언트에 상태가 없다는 의미가 아닌, **서로의 상태를 저장할 필요가 없다**는 의미이다. 서버는 **과거의 요청을 저장하지 않으며, 각 요청은 독립적으로 처리**된다. HTTP는 Stateless한 프로토콜이므로 HTTP를 활용하는 REST 역시 무상태성을 가진다. 

### 3. Cache(캐시)

요청에 대한 서버의 응답에 **데이터가 캐시 가능한지 불가능한지의 여부가 명시**되어 있어야 한다. 응답을 캐시 할 수 있다면 클라이언트에서 동일한 요청이 왔을 시에 응답 데이터를 재사용할 수 있어야 하고 HTTP Header에 cache-control을 이용한다.

### 4. Layered System(계층형 시스템)

서버는 다중 계층으로 구성될 수 있다. 클라이언트와 응답을 보내는 서버 간에 보안이나 캐싱 등 여러 서버 계층이 있는 형태이다. 하지만 중간 계층이 요청이나 응답에 영향을 미쳐서는 안 된다.

오늘날 대부분 HTTP만 잘 따라도 위 네가지 조건을 잘 지킬 수 있다.

### 5. Uniform Interface(균일한 인터페이스)

네 가지 제약 조건들로 구성된다.

- **자원의 식별**

  - 자원은 객체이고, 자원의 상태는 변화 가능하며 자원을 식별하기 위해 변하지 않는 식별자가 필요하다.
  - 서버의 개별 자원에 대한 고유한 식별자로 URI를 사용해야 한다.
  - 예를 들어, `/user/1`이라는 URI를 식별자로 사용하면 DB의 user 테이블의 특정한 데이터를 일관되게 지정 가능하다.

- **표현을 통한 자원의 조작**

  - 클라이언트가 자원을 요청할 때 서버는 자원 자체가 아닌 자원 표현으로 응답한다.
  - 예로 `GET /user/1 HTTP /1.1`은 `/user/1`에 대해 GET 요청을 서버에 날리면, 서버는 URI에 대응하는 사용자 1이라는 자원의 현재 상태를 HTTP로 표현하여 응답한다.
  - `POST /image`는 자원의 기대되는 초기 상태를 image로 표현하고, 이 표현을 담은 POST 요청을 서버로 보내면 서버에서는 이 표현을 바탕으로 새로운 이미지 자원을 생성하고 `/image/1`이라는 URI를 해당 자원에 대한 식별자로 할당한다.

- **자기 서술적인 메세지**

클라이언트와 서버 사이에는 수많은 컴포넌트들, 즉 중개자가 존재한다. 이때 클라이언트와 서버에서 보내는 요청 및 응답 메세지들은 중개자들에게 자신을 어떻게 처리해야 하는지에 대해 제대로 설명해야 한다. 즉, **메세지 스스로가 자신에 대해 설명할 수 있어야 한다는 것이다.**

`GET /user/1 HTTP/1.1`은 자기 서술적이지 못하다. 왜냐하면 목적지가 없기 때문이다. 요청 메세지의 경우, `GET /user/1 HTTP/1.1 Host: www.example.com` 과같이 host 헤더 필드에 도메인명(목적지)을 기재하여 어디로 보낸 요청이다에 대한 정보를 포함시켜주어야 한다. 

- **HATEOAS**

HATEOAS란 클라이언트는 서버와 상호작용하면서 하이퍼링크를 통해 동적으로 모든 다른 리소스에 접근할 수 있어야 한다는 것이다. **상태 전이**란 해당 URI에서 **사용자가 다음 행동으로 취할 수 있는 것**을 말한다. JSON 형식인 경우, Link 헤더를 통해 리소스를 가리키는 하이퍼링크를 타고 다른 상태로 전이가 가능하다. 모든 관련된 동작을 URI를 통해 알려주어, 클라이언트가 API의 변화에 일일이 대응하지 않아도 된다는 장점이 있다.
아래 예시는 HATEOAS를 만족한다. <a> 태그의 href 속성을 통해 하이퍼링크가 나와있고, 하이퍼링크를 통해 그 다음 상태로 전이가 가능하기 때문이다.

```
HTTP/1.1 200 OK
Content-Type: text/html

<html>
<head></head>
<body><a href="/test">test</a></body>
</html>
```

JSON의 Link 헤더를 통해 하이퍼링크와 연결되어 있는 다른 리소스를 가리킬 수 있다. 이전 게시물의 URI가 `/articles/1`이고, 다음 게시물의 URI가 `/articles/3`이라는 정보를 가지고 있다. Link 헤더를 통해 온전히 메세지를 해석하여 상태가 전이될 수 있다. URI를 통해 들어오는 요청을 예측할 수 있고, URI가 변경되더라도 클라이언트는 URI 변경에 따른 코드를 변경하지 않아도 된다.

```
HTTP/1.1 200 OK
Content-Type: application/json
Link: </articles/1>; rel=“previous”,
      </articles/3>; rel=“next”
```

정리하면 **REST API**는 **HTTP URI를 통해 자원을 명시하고 HTTP Method 표현을 통해 자원을 처리하도록 설계된 아키텍처**이다.

<br>

참고

[Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)

[그런 REST API로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc)

[우아한Tech RESTful](https://www.youtube.com/watch?v=NODVCBmyaXs)

