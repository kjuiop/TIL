### 레플리카

- 레플리카(Replica) 는 컴퓨터 시스템에서 데이터사 서비스를 복제하여 사용할 수 있도록 하는 기법
- 레플리카를 사용하면 시스템의 가용성을 높이고, 데이터나 서비스가 손상되지 않도록 보호할 수 있음
- 보통 데이터베이스를 Master, Slave 형태로 레플리카를 진행하기도 함
- Master
    - 데이터베이스의 쓰기 작업 (Insert, Update, Delete)을 담당합니다. 일반적으로 응용 프로그램은 Master 에 연결하여 쓰기 작업을 수행합니다.
- Slave
    - Slave 는 Master 데이터를 복제하여 사용합니다. Slave 는 일반적으로 데이터베이스의 읽기 작업 (Select) 을 담당합니다. 이를 통해 Master 에서 일어나는 쓰기 작업에 의한 성능 저하를 최소화할 수 있습니다.
- Master-Slave 구조는 단일 장애점(Single Point of Failure)을 최소화 하며, 읽기 요청을 분산하여 처리할 수 있어 성능을 향상시키는 장점이 있습니다. 그러나 이 구조는 쓰기 작업을 할 때 Master 에 과부하가 걸리기 쉬운 단점이 있습니다.


