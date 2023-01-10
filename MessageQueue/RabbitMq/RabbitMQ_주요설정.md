### RabbitMQ 주요 설정

- name
- durable : 메시지를 디스크에 저장, memory에 저장하는 것은 transient 라 한다.
- auto-delete
    - 모든 consumer 가 unsubscribe 하면, 해당 queue는 자동으로 없어진다.
    - Queue를 만드는 것을 declare 라 하며, 애플리케이션 코드에서도 쉽게 만들 수 있다.
    - 만약 해당 큐가 이미 존재하고 있다면, 다시 Queue 를 만들지 않고, Queue 가 없을 경우에만 만든다.
- exchange 타입
    - Default Exchange
    - Direct Exchange : 각 Queue 는 Routing Key 에 Binding이 되어있고, Exchange에 Routing key가 들어오면, 그 Exchange에 Binding 되어 있는 Queue 중에서, 그 Key와 Mapping 되어 있는 Queue 로 메시지를 라우팅한다.
    - Fan out Exchange : Routing Key 에 상관 없이 Exchange에 Binding 되어 있는 모든 Queue 에 메세지를 라우팅한다. (1:N 관계로, 모든 Queue에 메시지를 복제해서 라우팅한다.
    - Topic Exchange : Exchange 에 mapping 되어 있는 Queue 중에서 Routing key가 패턴에 맞는 Queue 로 모두 메시지를 라우팅한다.
    - Headers Exchange
- Binding : Exchange와 Queue 를 연결하는 것을 binding 이라고 하며, Binding 은 routing key와 Exchange type 을 attribute 로 optional로 동반한다. (routing key는 일종의 filter key처럼 동작한다.)
- connection : 물리적인 TCP Connection, 보안이 필요한 경우 TLS(SSL) Connection 을 사용할 수 있음
- channel : 하나의 물리적인 connection 내에 생성되는 가상 논리적인 connection 들, consumer 의 process 나 thread 는 각자 이 channel 을 통해서 queue 에 연결될 수 있다.
- virtual host: 웹 서버의 virtual host concept 와 같이, 하나의 물리적인 서버에 여러 개의 가상 서버를 만들 수 있다.
- message attributes and payload
- exclusive
    - 해당 Queue 를 혼자 쓸 것인지에 대한 여부이다.
    - true 인 경우 오직 하나의 subscriber 만 상룡할 수 있으며, 해당 subscriber 가 종료하면 Queue 는 삭제된다.
- nowait
    - async 모드로 동작할지에 대한 여부를 묻는 것이다.
    - 만약 false일 경우, 큐 선언 생성 함수는 서버에 큐를 생성하라 요청하고, 그에 대한 결과를 받은 이후에 함수가 종료된다.
    - 반대로 true 일 경우, 큐 선언 함수는 서버에 큐를 생성하라고 요청하고, 즉시 함수가 종료된다. 만약 문제가 발생하는 경우에는 channel 사용 도중에 에러가 발생하므로, 그냥 쓰면 된다.
- args
    - exchange 타입에서 설명한 header 타입일 때 사용하는 것과 Queue optinal 설정들을 하기 위한 테이블이다.
    - 헤더는 key:value 로 된다고 설명햇는데, Table 은 map[String]interface{}를 type한 것이다.

### 메세지 구조

- Header, Properties, byte[]data

### 트랜잭션 관리

- rabbit mq 는 XA 기반의 분산 트랜잭션은 지원하지 않음. local 단위에서 ack 받으면 성공처리함

# Reference

---

- [https://velog.io/@divan/Rabbitmq-with-Golang](https://velog.io/@divan/Rabbitmq-with-Golang)
- [https://pkg.go.dev/github.com/streadway/amqp#pkg-index](https://pkg.go.dev/github.com/streadway/amqp#pkg-index)
- [https://lob-dev.tistory.com/entry/RabbitMQ-Message-Queue-및-Message-보존-설정과-Fair-Dispatch](https://lob-dev.tistory.com/entry/RabbitMQ-Message-Queue-%EB%B0%8F-Message-%EB%B3%B4%EC%A1%B4-%EC%84%A4%EC%A0%95%EA%B3%BC-Fair-Dispatch)
- [https://kaizen8501.tistory.com/220](https://kaizen8501.tistory.com/220)

