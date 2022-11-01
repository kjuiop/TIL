# Spring 에서 DI 란 무엇일까?

---

## DI(Dependency Injection)

<br />

DI 란 스프링이 다른 프레임워크와 차별화 되어 제공하는 의존 관계 주입 기능으로, 객체를 직접 생성하는 것이 아니라 외부에서 생성한 후 주입시켜 주는 방식이다.

![1](https://user-images.githubusercontent.com/41246605/199237024-94787c44-63aa-482b-8046-6487502eff49.png)

첫번째 방법은 A 객체가 B와 C객체를 New 생성자를 통해서 직접 생성하는 방법이다.

두번째 방법은 외부에서 생성된 객체를 setter() 를 통해 사용하는 방법이다.

![2](https://user-images.githubusercontent.com/41246605/199237046-cb3c247c-df29-4159-8f75-5c411892c17f.png)


이러한 두번째 방식이 의존성 주입의 예시이다.

- A객체에서 B, C 객체를 사용(의존)할 때 A객체에서 직접 생성하는 것이 아니라 외부(IOC컨테이너)에서 생성된 B,C 객체를 조립(주입)시켜 setter 혹은 생성자를 통해 사용하는 방식이다.
- 스프링에서는 객체를 Bean 이라고 부르며, 프로젝트가 실행될 때 사용자가 Bean 으로 관리하는 객체들의 생성과 소멸에 관련된 작업을 자동적으로 수행해주는데, 객체가 생성되는 곳을 스프링에서는 Bean 컨테이너라고 부른다.


<br />

# IOC (Inversion of Control)

---

IOC 란 “제어의 역전” 이라는 의미로, 메소드나 객체의 호출작업을 개발자가 결정하는 것이 아니라 외부에서 결정되는 것을 의미한다.

객체의 의존성을 역전시켜 객체 간의 결합도를 줄이고, 유연한 코드를 작성할 수 있게 하여 가독성 및 코드 중복, 유지 보수를 편하게 할 수 있게 한다.

스프링이 모든 의존성 객체를 스프링이 실행될 때 다 만들어주고 필요한 곳에 주입시켜줌으로써 Bean들은 싱글턴 패턴의 특징을 가지게 된다.

제어의 흐름을 사용자가 컨트롤하는 것이 아니라 스프링에게 맡겨 작업을 처리하게 된다.

<br />

# Reference

---

- [https://velog.io/@gillog/Spring-DIDependency-Injection](https://velog.io/@gillog/Spring-DIDependency-Injection)
- [https://devmoony.tistory.com/43](https://devmoony.tistory.com/43)


<br />
