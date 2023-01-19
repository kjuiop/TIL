### Solid 원칙

- 단일 책임 원칙
    - 하나의 객체나 함수는 하나의 역할만 수행해야 하며, 다른 역할을 수행하면 안됩니다.

- 개방-폐쇄 원칙
    - 확장에는 열려있고, 변경에는 닫혀있어야 한다.
    - 쉽게 생각하면 if 문과 validate 문 차이
    - 인터페이스 추상화와 구현체 작성

```java
@FunctionalInterface
public interface SignUpService {
    String signUp(String uuid, MemberCommand.SignUp signUp);
}
```

```java
public SignUpService create(MemberCommand.SignUp signUpInfo) {

        switch (signUpInfo.getUsageAuthority()) {
            case MENTOR:
                return mentorSignUpService;
            case MENTEE:
                return menteeSignUpService;
            case PARENT:
                return parentSignUpService;
        }

        return null;
}
```

- 리스코프 치환 원칙
    - 상속되는 객체는 반드시 부모 객체를 완전히 대체해도 아무런 문제가 없도록 권고한다.
    - 자식이 상속받는 부모의 객체에게 영향을 받지 않도록 구성한다.
    - user, member, admin

- 인터페이스 분리 원칙
    - 클라이언트 입장에서 사용하는 기능만 제공하도록 인터페이스를 분리해야 한다.
    - Method 단위로 분리하는 것이 아닌, UseCase 별로 분리하는 것이다.

```java
public interface multifunction {
  void copy();
  void fax(Address from, Address to);
  void print();
}
```

```java
// ISP가 적용된 예제
public interface Print{
  void print();
}

public interface Copy {
  void copy();
}

public interface Fax {
  void fax(Address from, Address to);
}
```

```java
public class copyMachine implements Copy {
  @Override
  void copy() {
    System.out.println("### 복사 ###");
  }
}
```

- 의존성 역전 원칙

![1f](https://user-images.githubusercontent.com/41246605/213467675-f9c1e2e9-5343-4379-b0a3-fb89b5aab1ee.png)


- 어떤 경우에는 로봇 장난감을 가지고 놀고, 어떤 경우에는 자동차 장난감을 가지고 놀고, 어떤 경우에는 레고를 가지고 놀 수도 있다.
- 이때 실제 가지고 노는 구체적인 장난감은 변하기 쉬운 것
- 아이가 장난감을 가지고 노는 사실은 변하기 어려운 것
    - 인터페이스는 변하지 않는 것
    - 구현체 클래스는 변하기 쉬운 것