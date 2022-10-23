## HTTPS란?

HTTP는,

- 암호화하지 않은 통신으로 도청 가능
- 통신 상대를 확인하는 과정이 없으므로 위장 가능한 문제점이 있다.

**HTTPS**는 **SSL/TLS를 이용한 HTTP 통신**으로, **HTTP에 데이터 암호화가 추가**된 프로토콜이다. 

- **대칭키 암호화**
    - 클라이언트와 서버가 서로 동일한 키를 사용해 암호화/복호화를 진행
    - 키가 노출되면 위험하지만 연산 속도가 빠르다.
- **비대칭키 암호화**
    - 1개의 쌍으로 구성된 공개키와 개인키를 사용해 암호화/복호화를 진행
    - 키가 노출되어도 비교적 안전하지만 연산 속도가 느리다.

**비대칭키 암호화**에서 **공개키**와 **개인키**는 서로를 위한 1쌍의 키이다.

- **공개키** : 모두에게 공개가능한 키
- **개인키** : 타인에게 절대 노출되어서는 안되는 키

암호화를 공개키로 하느냐 개인키로 하느냐에 따라 다른 효과를 얻을 수 있다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Asymmetric%20key%20encryption.PNG)

- **공개키 암호화** : 공개키로 암호화를 하면 개인키로만 복호화할 수 있다. 제 3자는 데이터를 볼 수 없으므로 데이터가 안전하게 전송된다는 것을 보장한다.
- **개인키 암호화** : 개인키로 암호화를 하면 공개키로만 복호화할 수 있다. 암호화된 데이터가 공개키로 복호화된다는 것은 그 데이터가 공개키와 쌍을 이루는 개인키에 의해 암호화되었다는 것을 의미한다. 즉 공개키가 데이터를 제공한 사람의 신원을 보장해주게 되는 것이고, 이러한 것을 **전자서명**이라고 한다.

## CA

인증서의 역할은 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지 보장하는 역할을 한다. 이 역할을 하는 기업들을 CA라고 한다. CA를 브라우저는 알고 있고, 이 리스트는 브라우저에서 자체적으로 가지고 있다. SSL을 통해 암호화된 통신을 제공하려는 서비스는 CA를 통해 SSL 인증서를 구입해야 한다.

## SSL 

SSL은 전송되는 데이터를 암호화하는 계층으로, SSL은 SSL 인증서를 사용하며 SSL 인증서에는 다음 정보가 포함되어 있다.

1. 서비스의 정보(인증서를 발급한 CA, 서비스 도메인 등)
2. 서버 측 공개키

1번은 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지에 대한 내용을 담고 있고, 2번은 서버와 통신을 할 때 사용할 공개키 정보를 담고 있다. 위 내용은 CA에 의해서 암호화 된다.

- **SSL 인증서가 서비스를 보증하는 방법**
    - 클라이언트가 서버에 접속할 때 서버는 제일 먼저 SSL 인증서를 제공한다.
    - 브라우저는 이 SSL 인증서를 발급한 CA가 자신이 내장한 CA 리스트에 있는지 확인한다.
    - 확인 결과 서버를 통해서 다운로드한 SSL 인증서가 내장한 CA 리스트에 포함되어 있다면 해당 CA의 공개키를 이용해서 인증서를 복호화한다. **개인키와 공개키는 서로를 위해 1쌍으로 존재하고, 개인키로 암호화한 정보를 공개키로 복호화 할 수 있다는 것은 접속한 사이트를 신뢰를 보장해주게 되는 것이다.**

- **SSL 동작 방법**

SSL은 암호화된 데이터를 전송하기 위해 공개키와 대칭키를 혼합하여 사용한다. 클라이언트와 서버가 주고받는 실제 정보는 대칭키 방식으로 암호화하고, 대칭키 방식으로 암호화된 실제 정보를 복호화 할 때 사용할 대칭 키는 공개키 방식으로 암호화해서 클라이언트와 서버가 주고받는다.

## HTTPS 흐름

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/HTTPS%20Flow.PNG)

1. 클라이언트가 서버에 접속하는 단계를 Client Hello라고 한다. 이 단계에서는 주고받는 정보는 아래와 같다.
    - 클라이언트 측에서 생성한 랜덤 데이터
    - 클라이언트가 지원하는 암호화 방식들
    - 이전에 이미 Handshaking 기록이 있다면 자원 절약을 위해 기존 세션을 재활용하기 위한 세션 아이디
2. 서버는 Client Hello에 대한 응답으로 Server Hello를 하게 된다. 이 단계에서 주로 받는 정보는 아래와 같다. 
    - 서버 측에서 생성한 랜덤 데이터
    - 서버가 선택한 클라이언트 암호화 방식 
    - SSL 버전
3. 서버가  Ceriticate 메세지를 보낸다. 메세지에는 Server 의 SSL 인증서 내용이 들어있다.
4. 클라이언트는 서버의 SSL 인증서를 검증한다.
5. 클라이언트는 서버와 데이터를 안전하게 주고받기 위해 전달하는 데이터를 암호화해야 한다. 클라이언트는 데이터를 암호화하기 위한 대칭키를 생성한다. 클라이언트는 이 대칭키를 서버만 볼수 있도록 서버의 공개키로 암호화 하여 전달한다.
6. 서버는 클라이언트가 전달한 암호화된 대칭키를 받아 서버의 개인키로 복호화 한다. 이제 서버와 클라이언트는 모두 동일한 대칭키를 가지고 있으니 통신 준비가 끝났다는 Change Ciper Spec 메세지를 보낸다. 즉, 서버도 SSL Handshake 준비가 완료된 상태다.