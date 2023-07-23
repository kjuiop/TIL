# JWT Token 발급 절차

---

- Client 에서 ID/PW 를 통해 로그인을 요청한다.
- 서버에서 DB 에 해당 ID/PW 를 가진 User 가 있다면, Access Token, Refresh Token 을 발급해준다.
    - 이때 Access Token 은 짧게 (1시간정도) Refresh Token 은 길게 (6시간~12시간) 설정한다.
- 클라이언트는 발급받은 Access Token 을 헤더에 담아서 서버가 허용한 API 를 사용할 수 있게 된다.
- accessToken 으로만 로그인을 시도한다.
- accessToken 이 만료되었을 때 refresh token 으로 재발행을 한다.
    - ip 주소 확인도 괜춘
    - refresh token 도 access token 과 동일하게 생성했기 때문에, Authorization Header 로 받는다.
    - refresh token 에서 추출한 정보와 회원정보가 일치하는지 비교한다.
    - Refresh Token 을 통해 AccessToken 을 재발행한다면 그때 RefreshToken 도 재발행한다.
- 재발행된 accessToken 을 통해 다시 로그인을 한다.
- refresh token 의 경우에는 DB 에 저장하기도 합니다.
- refresh token 에서 username 을 파싱한 후 일치여부를 확인하고, 최초 로그인 시 발급받았던 토큰여부도 확인합니다.
    - 이유는 access token 으로 재발급받으려는 의도가 있을 수 있기 때문입니다.
- access token 의 경우에는 주기가 짧고, 매번 발급받기 때문에 부하 문제로 인해 저장하지 않습니다.