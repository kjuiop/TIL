
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


### Monolithic vs MicroService

- Monolith Architecture
  - 모든 업무 로직이 하나의 애플리케이션 형태로 패키지 되어 있는 서비스
  - 애플리케이션에서 사용하는 데이터가 한 곳에 모여 참조되어 서비스되는 형태
  - 특정 기능의 업데이트도 전체 빌드 및 배포가 필요함

![3](https://user-images.githubusercontent.com/41246605/206473525-48c74747-347c-45aa-b6df-fb4fa5efaf96.png)


- MSA
  - 각각의 서비스들은 최소한의 중앙집중식 관리
  - 서로 다른 프로그래밍 언어와 데이터 저장 방식을 사용할 수 있어야 한다
  - 도메인의 특징을 고려해서 각 서비스들이 구분되어야 하고, 각 서비스들은 독립적일 수 있어야 한다

![4](https://user-images.githubusercontent.com/41246605/206473547-77f12eea-d03f-460c-ab1e-b2a41eee7fa4.png)


![5](https://user-images.githubusercontent.com/41246605/206473563-b88f1afa-0db0-4d7c-93e4-35a55da7c082.png)


- Monolithic 방식은 UI, WebService, BusinessLogic, DatabaseLogic 을 하나의 구성요소로 보고 있음
- Micro Service
  - 서비스 요청에 대한 통일된 API Gateway
  - 트래픽 분산 ( 로드밸런서 )
  - 분리된 데이터의 통합
  - 각각의 서비스들은 서로 상태에 대해 RESTful 방식으로 통신을 함
  - 환경설정은 외부에서 컨트롤함 ( IP Address 등 ) - 매번 배포가 필요함
  - 동적으로 Scale up , down 이 가능해야 함
  - 자동화 배포관리 CI/CD 가 중요
  - 클라우드 환경에서 운영되어야 함

### MSA 개발 유의사항

- 서비스 인터페이스를 통해서만 갖고 있는 데이터와 함수를 공개함
- 각각의 팀들은 서비스 인터페이스를 통해서만 통신해야 한다. 직접 연결 X
- 각 팀들의 기술은 통일될 필요가 없음
- 예외없이 모든 서비스 인터페이스는 외부에 공개될 수 있도록 설계가 되어야 함

### 모든 서비스가 MSA 로 되어야 하나?

- 어느정도 변화가 생길 수 있을 것인가 ( 공수, 효과 )
- 독립 Life Cycles ( 각 모듈이 독립적으로 개발, 운영 배포 가능한가 )
- 독립적인 확장성 가능 여부
- 격리된 오류,  오류의 발생이 독립적인 문제로 될 수 있는지
- 외부 종속성과의 상호작용을 단순화시켜야 함, 응집력을 높일 수 있는가
- 여러 프로그래밍 언어를 사용할 수 있는 환경인가

### Microservice 와 SOA

- 서비스 공유 지향점
  - SOA - 재사용을 통한 비용 절감
  - MSA - 서비스 간의 결합도를 낮추어 변화에 능동적으로 대응

- 기술방식
  - SOA : 공통의 서비스를 ESB에 모아 사업 측면에서 공통 서비스 형식으로 서비스 제공
  - MSA : 각 독립된 서비스가 노출된 REST API 를 사용


### REST Ful API 설계 유의사항

- 소비자 입장에서의 설계 ( 간단 명료 )
- HTTP Method, Header 등 HTTP 의 장점을 최대한 살려야 함
- Request Method 활용
  - GET , POST , PUT , DELETE
- Response Status ( 응답 상태 코드 ) 규칙
  - 200, 404, 400, 201, 401, 500
- No secure info in URI
  - 비밀번호 등 보안이 필요한 데이터를 URI 에 쓰면 안됨
- 복수형태, 명사형태의 URI 값을 써야 함

![6](https://user-images.githubusercontent.com/41246605/206814435-c696ea65-69a4-4d6b-8940-183c7796730b.png)

<br />

![7](https://user-images.githubusercontent.com/41246605/206814440-810dca00-aed1-45d7-9294-7336b98a35cc.png)

<br />

- API Gateway 를 통해 필요한 서비스를 요청
- API Gateway 를 통해 수집된 요청은 Service Router 에게 어디로 가야할지 질문을 하게 됨
- Service Discovery 는 필요한 마이크로서비스가 어디에 저장되어 있는지 질문을 바음
- 마이크로 서비스는 각 A, B 에 인스턴스로 구성되어 있음
- Service Discovery 를 통해 필요한 마이크로서비스 위치를 알아냈으면 로드밸런서를 통해 해당 마이크로 서비스로 요청이 전달됨
- Service Router, load balancing 은 하나의 시스템으로 묶어서 사용되는 경우도 있음
- 마이크로서비스 환경 설정 정보는 Config Store 를 통해 외부 시스템에 저장시켜 사용함
- 마이크로서비스는 가상화 컨테이너 기반으로 구성되어 있음
- 완성왼 Application 은 CI/CD 를 통해 통합, 빌드, 배포 가 됨
- Backing System 은 마이크로 서비스의 저장되어있는 Storage를 모아서 사용되거나
  메시지 처리 시스템을 통해 하나의 서비스처럼 서비스들을 연결함
- telemetry 는 모니터링 기능이랑 진단기능을 갖고 있음


<br />

### Service Mesh

마이크로 서비스를 적용한 내부 통신, 인프라의 레이어라고 볼 수 있음

- MSA 인프라 → 미들웨어
  - 프록시 역할, 인증, 권한 부여, 암호화, 서비스 검색, 요청 라우팅, 로드 밸런싱
  - 자가치유 복구 서비스
- 서비스 간의 통신과 관련된 기능을 자동화
- 설정정보, 라우팅, 인증정보, 로드밸런싱, 탄력성, 서비스 검색, 암호화 등의 기능을 통해 MSA 운영을 지원
- 하나의 제품이 아닌 추상적인 개념

![8](https://user-images.githubusercontent.com/41246605/206814443-3997f586-efc4-430d-9c7d-d7eb438b82d8.png)

### Spring Cloud 란?

- 환경설정관리 (Centralized configuration management)
  - **Spring Cloud Config Server**
  - CurrencyCalculationService, CurrencyExchangeService, LimitsService
  - 게이트웨이의 IP, 서버의 토큰 기본정보 등을 Git 에 모아놓고 다른 MSA 에서 참조해서 사용


![9](https://user-images.githubusercontent.com/41246605/206814444-9594cf06-9005-4b90-8fe0-5968bf4b396c.png)


- 서비스 등록과 위치정보 확인 (Location transparency)
  - Naming Server (Eureka)

- 서버 요청 분산 (Load Distribution, Load Balancing)
  - Ribbon (Client Side)  : 게이트웨이 서비스
  - Spring Cloud Gateway


![10](https://user-images.githubusercontent.com/41246605/206814450-71d35cd9-afd8-4008-ac96-51a9bafc15c9.png)


- Easier REST Clients
  - FeignClient

- Visibility and monitoring
  - Zipkin Distributed Tracing
  - Netflix API gateway

- Fault Tolerance
  - Hystrix






