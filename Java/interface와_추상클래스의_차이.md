- 인터페이스와 추상 클래스는 객체 지향 프로그래밍에서 클래스들이 공통된 특성이나 기능을 공유할 수 있게 해주는 기능입니다.
- 인터페이스 (interface)
    - 인터페이스는 메서드 시그니처의 모음으로 기본 틀을 제공합니다.
    - 인터페이스를 구현하는 클래스는 인터페이스에 정의된 모든 메서드를 구현해야 합니다.
    - 인터페이스는 다중 상속을 지원합니다. 즉, 하나의 클래스가 여러 인터페이스를 구현할 수 있습니다.
    - 인터페이스는 코드의 재사용성을 높이고 다형성을 적용할 수 있게 합니다.
- 추상클래스 (abstract class)
    - 추상 클래스는 추상 메서드를 포함할 수 있습니다. 추상 메서드는 선언만 있고 구현이 없는 메서드입니다.
    - 추상 클래스를 상속하는 하위 클래스는 추상 메서드를 구현해야 합니다.
    - 추상 클래스는 다중 상속을 지원하지 않습니다.
    - 추상클래슨는 인터페이스와 달리 클래스의 구현 사항을 포함할 수 잇으므로, 코드 재사용성을 높이고 코드의 중복을 줄이는데 도움이 됩니다.

- 인터페이스와 추상 클래스의 선택 기준
    - 다중 상속이 필요한 경우에는 인터페이스를 사용합니다.
    - 기본 구현이 필요한 경우에는 추상 클래스를 사용합니다.
    - 공통된 특성이나 기능만 정의하고 실제 구현은 하위 클래스에게 맡기려면 인터페이스를 사용합니다.


- 인터페이스와 추상클래스의 차이
  - 인터페이스는 명시된 모든 메서드를 구현해야함
  - 추상클래스는 추상메서드와 일반 메서드를 포함할 수 있음.
    - 추상 메서드만 구현하면 됨
  - 인터페이스는 모든 메서드가 추상 메서드이며, 다중 상속을 지원함
  - 추상클래스는 추상 메서드와 일반 메서드를 포함할 수 있으며, 다중 상속을 지원하지 않음.



```java
abstract class Animal {
    // 추상 메서드
    abstract void makeSound();

    // 일반 메서드
    void sleep() {
        System.out.println("The animal is sleeping.");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("The dog barks.");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("The cat meows.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Cat cat = new Cat();

        dog.makeSound();
        cat.makeSound();
    }
}
```