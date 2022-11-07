# 웹 소켓

---

웹 소켓은 사용자의 브라우저와 서버 사이의 인터액티브 통신 세션을 설정할 수 있게 하는 고급기술입니다.

개발자는 웹 소켓 API를 통해 서버로 메시지를 보내고 서버의 응답을 위해 서버를 풀링하지 않고도 이벤트 중심 응답을 받는 것이 가능합니다.

웹소켓은 실시간 양방향 통신을 지원하며 한번 연결이 수립되면 클라이언트와 서버 모두 자유롭게 데이터를 보낼 수 있습니다. 이는 채팅과 같은 연속적인 통신에 대해 계속 유사한 통신을 반복하지 않게 해주어 통신의 효율성도 개선하였습니다.

웹소켓은 2011년 RFC 6455에 의해 표준화 되었으며, 이후 브라우저들에서 지원되기 시작하였습니다. Internet Explorer는 11버전 이후부터 지운하며, 현재는 대부분의 브라우저에서 지원합니다.

# 웹 소켓 프로토콜

---

웹 소켓은 HTTP 와 같은 OSI 모델의 7계층에 위치하는 프로토콜이며, 4계층의 TCP에 의존합니다.

HTTP 프로토콜을 이용할 때 `http` 를 이용하는 것처럼 웹소켓을 이용할 때 `ws` 를 이용합니다. 또한 보안을 강화한 `https` 를 사용하는 것처럼, `ws` 에 대해 보안을 강화한 `wss` 를 사용할 수 있습니다.

HTTP 를 이용해서 연결을 수립하며 연결된 이후에도 연결을 할 때 사용했던 포트인 80과 443포트를 이용합니다. 연결 수립은 핸드 쉐이크를 통해 이루어지며, 핸드쉐이크시 HTTP를 이용합니다.

![1](https://user-images.githubusercontent.com/41246605/200281807-e02bb9cd-0187-4897-8b68-106cdaab1739.png)


- 위 그림은 웹 소켓 핸드쉐이크와 통신을 보여주고 있습니다. 그림에서 볼 수 있듯이 핸드쉐이크는 한 번의 HTTP 요청과 HTTP 응답으로 이루어집니다. 핸드쉐이크가 끝나면 HTTP 프로토콜을 웹 소켓 프로토콜로 변환하여 통신을 하는 구조입니다.

- 핸드쉐이크는 먼저 클라이언트가 HTTP로 웹소켓 연결 요청을 하면서 시작됩니다. 웹 소켓 연결 요청에는 “Connection:Upgrade” 와 “Upgrade:websocket” 헤더를 통해 웹소켓 요청임을 표시합니다. 또한 “Sec-WebSocket-Key” 헤더를 통해 핸드쉐이크 응답을 검증할 키 값을 보냅니다. 그 외에도 WebSocket 연결 시 보조로 이용할 프로토콜 정보 등의 추가적인 정보를 헤더에 담아 보낼 수 있습니다.

### 웹 소켓 연결 요청 예시

```java
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```

- 이에 대한 서버의 응답 역시 HTTP로 옵니다. 정상적인 응답의 상태코드는 101(Switching Protocols) 입니다. “Sec-WebSocket-Key” 헤더를 통해 받은 값에 특정 값을 붙인 후, SHA-1 로 해싱하고 base64 로 인코딩한 값을 “Sec-WebSocket-Accept” 헤더에 보냅니다. 이 값을 통해 클라이언트는 정상적인 핸드쉐이크 과정을 검증합니다. 클라이언트가 정상적으로 응답을 받으면 핸드쉐이크는 종료되고, 이후부터 웹소켓 프로토콜을 통해 데이터 통신을 합니다.

### 웹 소켓 연결 응답 예시

```java
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```

- 이와 같이 핸드쉐이크가 끝나면 웹 소켓 프로토콜을 통해 텍스트를 주고 받을 수 있게됩니다.

# Reference

---

- [https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API](https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API)
- [https://tecoble.techcourse.co.kr/post/2020-09-20-websocket/](https://tecoble.techcourse.co.kr/post/2020-09-20-websocket/)


