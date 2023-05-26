우리가 어플리케이션을 테스트할 때에는 많은 고민이 있습니다. 하나의 요청에 대해 수많은 기능이 동작할 수 있는데, 이런 경우에는 과연 내가 만든 단위 기능이 올바르게 작동하는지 의문이 들 때가 있습니다.

하지만 하나의 덩어리로 강결합으로 엮어 있는 코드의 경우에는 테스트하기가 어려운데요.

이처럼 특정 부분만 테스트를 하고 싶을 때, Mock 이라는 객체를 만들어 사용합니다.

### Mock 객체란?

---

- 행위를 검증하기 위해 사용되는 객체를 지칭하며 수동으로 만들 수도 있고 프레임워크를 통해 만들 수도 있다.
- 행위 기반 테스트는 복잡도나 정확성 등 작성하기 어려운 부분이 많기 때문에 상태 기반 테스트가 가능하다면 만들지 않는다.
- Mock Object 분류
    - 행위 검증 (behavior verification)
    - 상태 검증 (state verification)