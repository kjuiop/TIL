## 자체 먼졉 질문

---

### 

문제 1)

세션과 쿠키의 차이점에 대해 설명하세요.

답 1)

쿠키는 클라이언트의 로컬에 저장되는 키와 값의 작은 데이터입니다. 텍스트 형식으로 저장되며, 브라우저 종료시에도 로컬에 남아있고, 도메인당 20개, 쿠키당 4kb 의 제한이 있습니다.

세션은 일정시간동안 같은 브라우저로 들어오는 일련의 요구사항을 하나의 상태로 보고 그 상태를 유지하는 기능입니다. 서버에 오브젝트 형식으로 저장되며 브라우저 종료 시 사라집니다.

---

문제 2)

객체지향과 절차지향의 차이점은 무엇인가요?

답 2)

절차지향 프로그래밍이란 순차적인 처리가 중요시되며, 프로그램 전체가 유기적으로 연결되도록 만드는 프로그래밍 기법입니다. 

컴퓨터의 처리구조와 유사해 실행속도가 빠르다는 장점이 있습니다.

반면 유지보수가 어렵고, 실행순서가 정해져있어 코드의 순서가 바뀌면 결과값이 달라질 수 있으며, 
디버깅이 어렵다는 단점이 있습니다.

---

문제 3)

객체지향 언어의 특징에 대해 설명하세요.

답 3)

캡슐화 : 관련된 데이터와 알고리즘이 하나의 묶음으로 정리된 것으로써 데이터를 감추고 외부세계와는 메소드를 통해 상호작용합니다.

상속 : 이미 작성된 클래스를 이어받아 새로운 클래스를 생성하는 기법으로 기존 코드를 재활용해서 사용합니다.

다형성 : 하나의 이름으로 많은 방법에 대처하는 기법

---

문제 4)

web server 와 was 의 차이를 설명하세요.

답 4)

web server : HTTP 프로토콜을 기반으로 하여 웹브라우저의 요청을 서비스하는 기능을 담당합니다. 예를 들어, apach server, Nginx 가 있습니다.

was : DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 어플리케이션 서버입니다. 예를 들어 tomcat 이 있습니다.

---

문제 5)

CDN 의 개념과 작동 원리에 대해 설명하세요.

답 5)

대용량 또는 사용자의 잦은 요청이 있는 컨텐츠를 Cache 서버에 분산 배치하여 컨텐츠의 전송 중 발생하는 트래픽 집중과 병목현상 및 데이터 손실을 해결하기 위해 등장한 컨텐츠 전송기술입니다.

작동원리

- 웹 브라우저가 실행되는 PC나 모바일 기기의 사용자 에이전트가 특정 주소에 접근하여 HTML, 이미지, CSS, JS 파일 등 렌더링하는데 필요한 컨텐츠를 서버로부터 요청합니다.
- DNS는 컨텐츠에 대한 각 요청이 발생하면 End User 와 가장 가까운 위치에 최적으로 배치된 CDN 서버에 End User 가 맵핑되고, 해당 서버는 요청된 파일의 캐싱된 버전으로 응답합니다.
- 서버가 파일을 찾는데 실패하는 경우 CDN 플랫폼의 다른 서버에서 콘텐츠를 찾은 다음 End User에게 응답합니다. 향후 요청에 응답할 수 있도록 patch 에 새로운 컨텐츠를 저장합니다.

---

문제 6)

웹 브라우저에 접속할 때 생기는 과정에 대해 설명해주세요.

답 6)

- 사용자가 브라우저에 URL 을 입력
- DNS 서버에 도메인 네임으로 서버의 진짜 주소를 찾음
- IP 주소로 웹 서버에 3 handshake로 연결 수립
- 클라이언트는 웹 서버로 HTTP 요청 메시지를 보냄
- 웹 서버는 HTTP 응답 메시지를 보냄
- 도착한 HTTP 응답 메시지는 웹 페이지 데이터로 변환되고, 웹 브라우저에 의해 출력

---

