## MicroService 의 특징

1. Challenges
    1. 마이크로서비스는 상당 수의 변화를 동반한다.
    2. 독립적으로 배포 가능한 단위로 분리되어야 하며, 각 서비스간 통신도 이루어져야 한다.
    3. 복잡도가 늘어난다.


2. Small Well Chosen Deployable Units
    1. 어플리케이션 도메인에 따라 서비스의 경계를 잘  구분해야 한다.


3. Bounded Context
    1. 하나의 서비스가 여러개의 서비스로 분리도 가능하고, 여러개의 서비스가 하나의 서비스로 통합도 가능해야 한다.
    2. 도메인 별 특정한 컨텍스트 하에서 완전한 의미를 갖으며 분리되는 것


4. RESTful
    1. 서로 상태에 대해서 REST 방식으로 통신해야 한다.
    2. HTTP 프로토콜, JSON Format


5. Configuration Management
    1. 마이크로서비스가 가지고 있는 환경변수나 설정에 대한 정보는 코드 내가 아닌 외부 시스템에서 관리되어야 한다.


6. Cloud Enabled
    1. Cloud 환경으로 최대한 구현


7. Dynamic Scale Up And Sacle Down
    1. 동적으로 Scaleup/Down 가능해야함
    2. 시각화할 수 있는 관리도구도 가지고 있어야 한다.


8. CI/CD
    1. 자동화배포


9. Visibility
    1. 시각화


