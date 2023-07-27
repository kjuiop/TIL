# Getter 와 Setter 를 지양하는 이유

---

### 접근제한자란

- public : 모든 접근을 허용
- protected : 같은 패키지(폴더)에 있는 객체와 상속관계의 객체들만 허용
    - 자식 클래스는 사용 가능
- default : 같은 패키지(폴더)에 있는 객체들만 허용
- private : 현재 객체 내에서만 허용

### Builder 패턴이란

- 빌더 패턴은 복잡한 객체들을 단계별로 생성할 수 있도록 하는 생성 디자인 패턴입니다.

```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade;
    private String phoneNumber;

    public StudentBuilder id(int id) {
        this.id = id;
        return this;
    }

    public StudentBuilder name(String name) {
        this.name = name;
        return this;
    }

    public StudentBuilder grade(String grade) {
        this.grade = grade;
        return this;
    }

    public StudentBuilder phoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
        return this;
    }
}
```

이 빌더패턴에서 알 수 있듯이 return this 를 통해 값을 주입한 빌더를 계속해서 리턴합니다.

우리는 이 builder 패턴을 통해 조립의 형태로 값을 넣을 수 있는 것입니다.

자바에서는 클래스를 작성할 때 모든 필드를 private 으로 숨기고, public 메소드를 통해 간접적으로 필드를 다루게 한다.

그러나 getter, setter 메서드로 해당 필드를 호출하게 되면, 클래스 내부적으로 어떤 필드를 쓰는지 외부에 노출되게 되고, 하나의 기능을 만드는데 setter 와 getter 를 여러번 쓰게 되서 서비스 로직에서 가독성이 떨어지게 된다.

우리가 객체지향으로 프로그래밍하는 것은 객체가 어떤 작업을 하는지 네이밍을 통해 만들어놓는 것이 중요하지, 각 필드별 get/set 을 사용할 필요가 없다.

특히 Setter 를 사용하게 되면 Request Param 으로 넘어온 정보가 어디서부터 바뀌는지 모르게 되기 때문에 우리는 디버그를 통해 요청의 흐름을 일일이 살펴봐야 한다.

또한 Setter 를 사용하는 순간 그 Setter Field 에 대한 Validation 역시 객체 안이 아닌, Service Logic 에서 사용되기 때문에 로직이 분산될 위험이 있다.

그래도 Setter 사용자체를 지양하고, 정말 필요한 순간에만 사용한 것으로 보인다.