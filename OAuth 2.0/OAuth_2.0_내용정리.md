## OAuth 2.0 이란?

---

OAuth 2.0 (Open Authorization 2.0, OAuth2) 은 인증을 위한 개방형 표준 프로토콜

Third-Party 프로그램에게 리소스 소유자를 대신하여 리소스 서버에서 제공하는 자원에 대한 접근 권한을 위임하는 방식을 제공합니다.

구글, 페이스북, 카카오, 네이버 등에서 제공하는 간편 로그인 기능도 OAuth2 프로토콜 기반의 사용자 인증 기능을 제공하고 있습니다.

### 주요용어

| Authentication | 인증, 접근 자격이 있는지 검증하는 단계를 말합니다. |
| --- | --- |
| Authorization | 인가, 자원에 접근할 권한을 부여하는 것입니다. 인가가 완료되면 리소스 접근 권한이 담긴 Access Token 이 클라이언트에게 부여됩니다. |
| AccessToken | 리소스 서버에게서 리소스 소유자의 보호된 자원을 획득할 때 사용되는 만료 기간이 있는 Token 입니다. |
| RefreshToken | Access Token 만료 시 이를 갱신하기 위한 용도로 사용하는 Token 입니다. Refresh Token 은 일반적으로 AccessToken 보다 만료 기ㅏㄱㄴ이 깁니다. |

### OAuth 2.0 의 요소

| 구분 | 설명 |
| --- | --- |
| Client | OAuth 2.0을 사용해 서드파티 로그인 기능을 구현할 자사 또는 개인 애플리케이션 서버 |
| Resource Owner | 서드파티 애플리케이션 (Google, Facebook, Kakao 등)에 이미 개인정보를 저장(회원가입) 하고 있으며 Client 가 제공하는 서비스를 이용하려는 사용자
’Resource’ 는 개인정보라 생각하면 된다. |
| Resource Server | 사용자의 개인정보를 가지고 있는 애플리케이션 (Google, Facebook, Kakao 등) 서버이다. Client 는 Token 을 이 서버로 넘겨 개인정보를 받을 수 있다. |
| Authorization Server | 권한을 부여 (인증에 사용할 아이템을 제공해주는) 해주는 서버이다.
사용자는 이 서버로 ID, PW를 넘겨 Authorization Code를 발급 받을 수 있다.
Client 는 이 서버로 Authorization Code 를 넘겨 Token 을 발급 받을 수 있다. 


<br />

![1](https://user-images.githubusercontent.com/41246605/209637572-8a290e42-bb80-42d9-8fa3-3f8affbbdbde.png)

### JWT 와 OAuth 2.0 의 차이

- OAuth 2.0 은 하나의 플랫폼의 권한 (아무 의미없는 무작위 문자열 토큰)으로 다양한 플랫폼에서 권한을 행사할 수 있게 해줌으로써 리소스 접근이 가능하게 한다.
- JWT 는 Cookie, Session 을 대신하여 의미있는 문자열 토큰으로써 권한을 행사할 수 있는 토큰의 한 형식이다.

<br />

![2](https://user-images.githubusercontent.com/41246605/209637580-fcfb9b6d-d2c9-4b01-ad8d-8712f193ae85.png)
