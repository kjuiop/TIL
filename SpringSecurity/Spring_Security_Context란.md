# Spring Security Context

---

![1a](https://user-images.githubusercontent.com/41246605/215015140-86e3a53a-6aeb-4369-af41-dfc88dd2d1b3.png)


- Spring Security Context 는 Spring Framework 에서 인증 정보를 저장하는 공간입니다.


- 이 공간은 SecurityContextHolder 클래스를 통해 접근할 수 있으며, SecurityContextHolder는 thread-local storage 를 이용하여 관리합니다.


- SeucirtyContextHolder 는 현재 스레드에서 인증 정보를 가져오거나 설정할 수 있는 메서드를 제공합니다. SecurityContext 에는 Authentication 객체가 저장되며, Authentication 객체는 인증된 사용자 정보를 가지고 있습니다.
    - 이를 통해 인증된 사용자의 권한, 이름, 아이디 등을 알 수 있습니다.


- Spring Security 는 인증 정보를 저장할 때 JWT 인증을 사용할 수 있으며, 인증정보를 저장하는 공간인 Security Context 를 thread-local storage 를 이용하며 관리하며, 요청 종료 시 인증 정보는 삭제됩니다.


- thread-local storage 는 특정 스레드에 할당된 메모리 공간으로, 해당 스레드가 종료되면 SecurityContext 에 있던 인증 정보는 thread-local storage에 할당된 메모리 공간에서 제거됩니다.



### JWT 로그아웃 기능 구현

- Redis 는 RefreshToken 을 저장하는 용도로만 사용되었지만, 로그아웃 과정에서는 추가로 AccessToken 을 BlackList 로 저장하는 기능이 추가됩니다.
- 로그인 검증과정에서 AccessToken 을 Redis 에서 관리하면 체크를 너무 빈번하게 관리하게 됨.
- Redis에서 AccessToken 을 삭제하거나, BlackList 기능을 사용
  - AccessToken 의 유효시간
  - 로그아웃 성공된 회원의 AccessToken 을 BlackList 로 Redis 에 등록할 때, 토큰 값 자체를 key로 두고, value 로 logout 이라는 값을 주었습니다.
  - 블랙리스트로 등록하는 로그아웃된 액세스 토큰으로 요청이 들어왔을 때, 해당 토큰의 유효성이 남아있는 동안은 Redis 에 해당 토큰 값이 key 로 블랙리스트로 등록되어 있을 것이기 때문에 로그인을 할 수 없습니다.
  - Redis 로그아웃 로직
    - 먼저 요청받은 AccessToken 유효성 검증
    - 유효성 검증이 끝나고 액세스 토큰을 통해 Authentication 객체를, 그리고 저장된 user email 을 가져옵니다.
    - user email (redis key) 값을 통해 저장된 Refresh Token 이 있는지 여부를 검사합니다.
    - 요청으로 들어온 액세스 토큰의 유효시간을 가져와서 해당 액세스 토큰을 키 값으로 하고, 유효시간을 적용시켜 Redis 에 블랙리스트로 등록합니다.
- AccessToken 이 로그아웃된 토큰인지 검사를 위해 Redis Connection 을 가져가야 하므로, Front 단에서 처리함.
- 보통은, Redis 의 Refresh Token 만을 삭제함.


