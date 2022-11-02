# MockMvc 란?

---

실제 객체와 비슷하지만 테스트에 필요한 기능만 가지는 가짜 객체를 만들어서 애플리케이션 서버에 배포하지 않고도 스프링 MVC 동작을 재현할 수 있는 클래스를 의미한다.

즉, 실제 객체와 비슷하지만 테스트에 필요한 기능만 가지는 가짜 객체를 만들어서 서버에 배포하지 않고도 스프링 MVC 동작을 재현할 수 있는 클래스를 의미한다.

## MockMvc 필요성

---

객체를 테스트하기위해서는 테스트 대상 객체가 메모리에 있어야 하는데, 생성하는데 복잡한 절차가 필요하거나 많은 시간이 소요되는 객체가 있을 수 있고, 웹 어플리케이션의 Controller 처럼 WAS나 다른 소프트웨어의 도움이 필요한 객체도 있을 수 있다.

이러한 복잡한 객체를 테스트하기 위해 실제 객체와 비슷한 가짜 객체를 만들어 테스트에 필요한 기능만 가지도록 모킹하면 테스트가 쉬워진다.

- @WebMvcTest
    - 웹에서 테스트하기 힘든 컨트롤러를 테스트 하는데 적합하다.
    - 단순히 Web Controller 의 Request, Response 에 대한 검증이나, View, Error 에 대한 검증만을 할 때 사용된다.
- @AutoConfigureMockMvc
    - 컨트롤러 뿐만 아니라 테스트 대상이 아닌 @Service , @Repository 붙은 객체들도 모두 메모리에 올립니다.
    - MockMvc 를 보다 세밀하게 제어하기 위해 사용하며, 전체 애플리케이션 구성을 로드하고, MockMvc 를 사용하려는 경우 @AutoConfigureMockMvc + @SpringBootTest 를 고려해야 한다.

| 종류 | 요약 | Bean 범위 |
| --- | --- | --- |
| @SpringBootTest | 전체 테스트 어노테이션 | 애플리케이션에 주입된 Bean 전체 |
| @WebMvcTest | Controller Layer 테스트 | MVC 관련 Bean (Controller, Service) |
| @DataJpaTest | Jpa (DB I/O) 테스트 | JPA 관련 Bean (EntityManager) |
| @RestClientTest | Rest API 테스트 | RestTemplate 등 일부 Bean |
| @JsonTest | Json 데이터 테스트 | Json 관련 일부 Bean |

# MockMvc 메소드

---

### perform()

- 요청을 전송하는 역할을 합니다.
- 결과로 ResultActions 객체를 받으며, ResultActions 객체는 리턴 값을 검증하고 확인할 수 있는 andExpect() 메소드를 제공합니다.

### get(”/mock/blog”)

- HTTP 메소드를 결정할 수 있습니다.
    - get(), post(), put(), delete()
- 인자로는 경로를 보내줍니다.

### params(info)

- 키=값의 파라미터를 전달할 수 있습니다.
- 여러 개일 때는 params()를, 하나일 때에는 param()을 사용합니다.

### andExpect()

- 응답을 검증하는 역할을 합니다.
- 상태코드 ( status() )
    - 메서드 이름 : 상태코드
        - isOk() : 200
        - isNotFound() : 404
        - isMethodNotAllowed() : 405
        - isInternalServerError() : 500
        - is(int status) : status 상태 코드
    - 뷰 ( view() )
        - 리턴하는 뷰 이름을 검증합니다.
    - 리다이텍트 ( redirect() )
        - 리다이렉트 응답을 검증합니다.
    - 모델 정보 ( model() )
        - 컨트롤러에서 저장한 모델들의 정보 검증
    - 응답 정보 ( content() )
        - 응답에 대한 정보를 검증해줍니다.

### andDo(print())

- 요청/응답 전체 메세지를 확인할 수 있습니다.


# Reference

---

- [https://shinsunyoung.tistory.com/52](https://shinsunyoung.tistory.com/52)
- [https://velog.io/@boo105/MockMvc-넌-또-뭐하는-애야](https://velog.io/@boo105/MockMvc-%EB%84%8C-%EB%98%90-%EB%AD%90%ED%95%98%EB%8A%94-%EC%95%A0%EC%95%BC)
- [https://blog.neonkid.xyz/272](https://blog.neonkid.xyz/272)
- [https://spring.io/guides/gs/testing-web/](https://spring.io/guides/gs/testing-web/)
- [https://docs.spring.io/spring-boot/docs/1.4.2.RELEASE/reference/html/boot-features-testing.html](https://docs.spring.io/spring-boot/docs/1.4.2.RELEASE/reference/html/boot-features-testing.html)

<br />