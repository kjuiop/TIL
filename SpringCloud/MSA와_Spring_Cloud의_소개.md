
# Microservice 란?

---

### Microservice History

- 1960 ~ 1980s : Fragile, Cowboys
    - Mainframe, Hardware


- 1990 ~ 2000s : Robust, Distributed
    - Changes


- 2010s ~ : Resilient/Anti-Fragile, Cloud Native
    - Flow of value 의 지속적인 개선

<br />

→ 과거에는 하드웨어 자체가 고가이며, 시스템 변경이 어려웠으나 1990년도 들어가면서 점차 시스템이 분산화되면서 안정성을 띄게 됨

→ 2010년부터는 시스템의 환경이 로컬에서 클라우드로 환경이 변화되었으며, 안정성과 확장성이 비약적으로 발전함

→ DevOps 라는 직무와 Cloud Architecture 라는 기술이 생겨나게 됨

<br />

### Antifragile 의 특징

![1](https://user-images.githubusercontent.com/41246605/206187700-44658426-f897-4d7f-93a2-1ca7593fa881.png)

- Auto scaling
  - Auto Scaling Group
    - Desire capacity : 수동조절
    - Min : 최소 스케일 수
    - Max : 최대  스케일 수


- Microservices
  - 전체 서비스를 구축하는 개별적인 모듈을 독립적으로 개발, 배포, 운영하는 것
  - 예시로 넷플릭스, AWS 가 있음


- Chaos engineering
  - 시스템이 급격한 예측 못한 상황이라도 견딜 수 있고, 신뢰성을 쌓을 수 있는 소프트웨어 시스템의 실행하는 방법이나 규칙
  - 안정적인 시스템 운영


- Continuous deployments
  - CI/CD 와 같은 배포 파이프라인
  - 지속적인 배포 및 통합
  - 수많은 마이크로서비스의 애플리케이션의 배포 및 통합의 자동화

<br />

## Cloud Native Architecture

---


- 확장 가능한 아키텍처
  - 시스템의 수평적 확장에 유연
    - Scale Up , Scale Out 가능, Auto Scaling 가능
  - 확장된 서버로 시스템의 부하 분산, 가용성 보장
  - 시스템 또는 서비스 애플리케이션 단위의 패키지 (컨테이너 기반 패키지)
  - 모니터링


- 탄력적 아키텍처
  - 서비스 생성 - 통합 - 배포, 비즈니스 환경 변화에 대응 시간 단축
    - CI/CD 자동화된 배포로 인해 단축 시킬 수 있음
  - 분할된 서비스 구조
    - 서비스의 경계에 따라 구분하여 개발이 필요함
    - 각 서비스 간의 종속성을 최소화가 필요함
  - 무상태 통신 프로토콜
    - 상태를 갖지않는 서비스를 제공해야 함
  - 서비스의 추가와 삭제 자동으로 감지
    - 자신이 배포될 때 자신의 위치가 어디인지 등록을 해야 함
    - 타 서비스에서 해당 서비스가 검색 가능
  - 변경된 서비스 요청에 따라 사용자 요청 처리 (동적 처리)


- 장애 격리 (Fault isolation)
  - 특정 서비스에 오류가 발생해도 다른 서비스에 영향을 주지 않음


- 지속적인 통합, CI (Continuous Integration)
  - 통합 서버, 소스 관리 (SCM) , 빌드 도구, 테스트 도구
  - ex) Jenkins, Team CI, Travis CI


- 지속적 배포
  - Continuous Delivery
    - 소스 및 실행파일 수동 반영
  - Continuous Deployment
    - 소스 및 실행파일 자동 반영
  - Pipe line

<br />

- Container 가상화

![2](https://user-images.githubusercontent.com/41246605/206188733-2fcd3acd-e54f-4f82-bcc0-1f92a81433ef.png)


  - 기존의 로컬환경에서 운영했던 시스템을 클라우드 환경에서 운영한 기술
    - 전통적 운영체제
      - 하드웨어 위에 운영시스템을 올려 APP 가동
    - 가상화 운영체제
      - 운영체제 위에 Hypervisor 기술을 통해 가상머신을 기동함.
      - 각각의 가상머신은 독립적인 운영체제를 가짐
      - 호스트 운영체제에 많은 부하를 줌
    - 컨테이너 가상화 기술
      - 운영체제 위에 컨테이너 가상화 서비스를 사용하여 운영체제는 같이 사용하지만 APP 은 각각 독립적으로 시행



## 12-Factors

---


- BASE CODE
  - 코드 통합 및 통일적인 관리
  - 단일 코드 베이스, 자체 레파지토리에 저장된 각 마이크로서비스 소스에 대한 버전관리


- Dependency Isolation
  - 종속성의 배제
  - 각 모듈별로 변경될 수 있어야 함


- Configurations
  - 환경설정의 외부관리


- linkable backing services
  - 보조 서비스 지원


- stages of creation
  - 빌드, 릴리즈, 실행 환경을 분리하는 것
  - 각각은 고유의 아이디로 태그를 갖고 있어야 함
  - 롤백 기능을 지니고 있어야 함
  - CI/CD 를 이용하여 자동화 시켜야 함


- stateless processes
  - 각 서비스는 모두 분리된 채 운영될 수 있어야 함


- port binding
  - 자체 포트에서 노출되는 인터페이스 및 기능이 있어야 함


- concurrency
  - 동시성
  - 하나의 서비스가 여러 인터페이스에 동일하게 수행될 수 있어야 트래픽을 분산해도 동일한 기능을 제공할 수 있음


- disposability
  - 서비스 인스턴스 자체가 삭제 가능해야 한다.
  - 확장과 삭제가 자연스러워야 한다.


- development & production parity
  - 개발과 운영환경의 분리


- logs
  - 로그를 이벤트 스트림으로 해야함. 어플리케이션이 작동하지 않아도 로그는 계속 작동되어야 함
  - ELK, 등 로그관리시스템을 별도로 도입


- admin process for eventual processes
  - 관리도구가 필요함 ( 전체 MSA 시스템 현황을 알 수 있어야 함 )


- API First


- Telemetry


- Authentication and authorization
  - 인증정보


