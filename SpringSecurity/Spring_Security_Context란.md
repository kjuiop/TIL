# Spring Security Context

---

![1a](https://user-images.githubusercontent.com/41246605/215015140-86e3a53a-6aeb-4369-af41-dfc88dd2d1b3.png)


- Spring Security Context 는 Spring Framework 에서 인증 정보를 저장하는 공간입니다.


- 이 공간은 SecurityContextHolder 클래스를 통해 접근할 수 있으며, SecurityContextHolder는 thread-local storage 를 이용하여 관리합니다.


- SeucirtyContextHolder 는 현재 스레드에서 인증 정보를 가져오거나 설정할 수 있는 메서드를 제공합니다. SecurityContext 에는 Authentication 객체가 저장되며, Authentication 객체는 인증된 사용자 정보를 가지고 있습니다.
    - 이를 통해 인증된 사용자의 권한, 이름, 아이디 등을 알 수 있습니다.


- Spring Security 는 인증 정보를 저장할 때 JWT 인증을 사용할 수 있으며, 인증정보를 저장하는 공간인 Security Context 를 thread-local storage 를 이용하며 관리하며, 요청 종료 시 인증 정보는 삭제됩니다.


- thread-local storage 는 특정 스레드에 할당된 메모리 공간으로, 해당 스레드가 종료되면 SecurityContext 에 있던 인증 정보는 thread-local storage에 할당된 메모리 공간에서 제거됩니다.