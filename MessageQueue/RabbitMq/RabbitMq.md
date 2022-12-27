# RabbitMq 란

---

## RabbitMQ 동작 이해하기

비동기 작업 큐 (예: Celery) 를 사용하려면 중간 단계에 Broker 라고 부르는 메시지 큐가 항상 등장한다.
메시지 큐에는 RabbitMQ, ActiveMQ, ZeroMQ, Kafaka 등이 대표적이다.

- RabbitMQ 용어
  - Producing : 메시지를 보내는 프로그램
  - Queue : 메시지를 담고 있는 저장소
    - 메시지는 RabbitMQ 와 애플리케이션을 통해 흐르지만 Queue 내부에만 저장 가능
    - 많은 생산자가 하나의 대기열로 이동하는 메시지를 보낼 수 있음
    - 많은 소비자가 하나의 대기열에서 데이터 수신을 시도할 수 있음
  - Consuming : 메시지 수신을 기다리는 프로그램
- Producing, Queue, Consuming 은 동일한 호스트에 있을 필요가 없음


## AMQP

클라이언트가 메시지 미들웨어 브로커와 통신할 수 있게 해주는 메세징 프로토컬이다.

![2](https://user-images.githubusercontent.com/104340102/205767296-da3fccf2-6eef-438a-8f45-07bcc78898de.png)

메시지를 받아가는 Consumer 에서는 브로커의 Queue 를 통해 메시지를 받아가서 처리한다.

AMQP 에는 네트워크에 문제가 있거나, 메시지를 처리하지 못하는 경우를 대비해 2가지 수신확인
모델을 가지고 있다.

하나는 Consumer 가 메시지를 받으면 명시적으로 Broker 에게 통지하고, 브로커는 이 알림을 받았을 때만 Queue 에서 메시지를 삭제한다. 다른 하나는 Broker 가 메시지를 전달하면 자동으로 삭제하는 방식이다.

<br />

![1](https://user-images.githubusercontent.com/104340102/205767350-4aa5557d-323d-4b88-b967-87e5fc3a3aba.png)

- Rabbitmq 는 기본적으로 amqp 를 사용한다.
- Rqbbitmq 의 구성은 메시지큐와 Exchange 가 있다.
- 메시지를 publish 할 publisher는 오직 exchange를 통해 publish 할 수 있다.
- consumer 는 별다른 매개체 없이 그냥 큐를 직접 consume 한다.
- Exechange 는 4개의 종류가 있다.

<br />

### Exchange 와 Queue 를 연결하는 Bindings

- 모든 메시지는 Queue 로 직접 전달되지 않고, 반드시 Exchange 에서 먼저 받는다.
- Exchange Type 과 Binding 규칙에 따라 적절한 Queue 로 전달된다.
  - Name : Exchange 이름
  - Type : 메시지 전달 방식
    - Direct Exchange
    - Fanout Exchange
    - Topic Exchange
    - Headers Exchange
  - Durability : 브로커가 재시작 될 때 남아 있는지 여부
    - durable
    - transient
  - Auto-delete : 마지막 Queue 연결이 해제되면 삭제

<br />

### Bindings

- 생성된 Exchange 에는 전달 받은 메시지를 원하는 Queue 로 전달하기 위해 Bindings 이라는 규칙을 정의할 수 있음
- 간단하게 목적지 Queue 이름만으로도 Binding 을 추가할 수 있고, 일부 Exchange type 에 따라 routing key 를 지정해서 멧지ㅣ를 필터링한 후 지정한 Queue 로 보내도록 정의할 수 있다.


## RabbitMQ 특징

- 기본적으로 메시지 큐(Message Queue) 서버이기 때문에 메시지가 누락될 위험이 거의 없다.
- 연결이 끊어졌다 하더라도 다시 연결하는 순간 큐에 저장된 메시지를 구독할 수 있으며, 순서 역시 보장된다.
- 내결함성(Fault tolerance)이 극도로 뛰어납니다. 이는 Erlang 언어의 특징
- 외부 의존성이 없다.
- 동시성 성능이 매우 뛰어나다. Erlang 언어의 특징
- 수평적 확장이 쉽다. 여러 RabbitMQ 서버를 클러스터로 묶을 수 있다.
- 초당 수만건의 메시지는 큰 문제 없이 전송이 가능하다. Kafka 정도의 성능은 아니지만 실 사용에는 문제가 없다.
- WebSocket/STOMP 등의 웹 기반 소켓 프로토콜 역시 지원한다.
- RabbitMQ 다양한 언어로 라이브러리를 제공한다.

## 메시지를 보관하는 Queue

- Consumer 어플리케이션은 Queue 를 통해 메시지를 가져간다.
- Queue 는 반드시 미리 정의해야 한다.
  - Name : queue 이름. `amq.` 로 시작하는 이름은 예약되어 사용할 수 없다.
  - Durability : `durable` 은 브로커가 재시작 되어도 디스크에 저장되어 남아있고, `transient` 으로 설정하면 브로커가 재시작되면 사라진다. 단 Queue 에 저장되는 메시지는 내구성을 갖지 않는다.
- Auto delete : 마지막 Consumer 가 구독을 끝내는 경우 자동으로 삭제된다.
- Arguments : 메시지 TTL, Max Length 같은 추가 기능을 명시한다.

## 하나의 연결을 공유하는 Channel

- Consumer 어플리케이션에서 Broker 로 많은 연결을 맺는 것은 바람직하지 않다.
- RabbitMQ 는 Channel 이라는 개념을 통해 하나의 TCP 연결을 공유해서 사용할 수 있는 기능을 제공한다.
- 멀티 스레드, 멀티 프로세르를 사용하는 작업에서는 각각 별도의 Channel 을 열고 사용하는 것이 바람직하다.

### 환경 분리를 위한 Virtual Hosts

- 하나의 Broker 에서 운영 환경(ex. live, dev) 에 따라 Users, Exchange, Queue 등을 각각 사용할 수 있는 Vhosts 컨셉을 가지고 있다.

### Message Queue 및 Message 보존

- RabbitMQ server 가 종료 후 재기동하면, 기본적으로 Queue 는 모두 제거됩니다.
- 이를 막기 위해서는 Queue 를 생성할 때, Durable 옵션에 true 를 주고 생성해야 하며, Producer가 메시지를 발송할 대 PERSISTENT_TEXT_PLAIN 옵션을 주어야 메시지가 보존됩니다.

### Prefetch Count

- 하나의 Queue 에 여러 Consumer 가 존재할 경우, Queue 는 기본적으로 Round-Robin 방식으로 메시지를 분배합니다.
- 그런데 Consumer 가 2개인 상황에서 홀수번째 메세지는 처리 시간이 짧고, 짝수번째 메세지는  처리시간이 매우 긴 경우, 계속해서 하나의 Consumer 만 일을 하게 되는 상황이 발생할 수 있습니다.
- 이를 예방하기 위해 prefetch count 를 1로 설정해두면, 하나의 메시지가 처리되기 전(Ack 를 보내기 전)에 새로운 메시지를 받지 안헥 되므로, 작업을 분산시킬 수 있습니다.

### Dispatching

- 여러 소비자가 1개의 Queue를 바라보고 있다면 RabbitMQ 에서는 Round-robin 을 사용해 메시지를 균등하게 분배합니다.
- 중복 처리를 방지하기 위해 첫 번째 메시지는 소비자 1에게 전달하고, 두번째 메시지는 소비자 2에게 전달합니다.
- 소비자 뿐만 아니라 여러 생산자도 같은 Queue 에 메시지를 전달할 수 있습니다.
- 프로그램의 수평확장이 가능합니다.

### Fair dispatching

- 소비자는 2개만 존재하고 홀수번째의 메시지의 크기는 항상 크고, 짝수번째 메시지는 항상 작다면 Round-robin 알고리즘을 사용해서 메시지 분배를 해도 공평하게 소비자에게 전달되지 않습니다.
- 지연이 발생한 소비자에는 메시지를 전달하지 않도록 prefetch count 라는 개념을 사용합니다.
- prefetch count 가 1로 설정되어있다면 소비자로부터 ack 을 받지 못한 메시지가 1개라도 있으면 해당 소비자에게 메시지를 전달하지 않습니다.
- prefetch count 는 소비자에게 동시에 전달되는 메시지의 양입니다.

## 소비자 서버가 죽었을 경우

- Queue 는 소비자에게 메시지를 전달한 후 ACK 을 받았을 때, 해당 메시지를 dequeue 합니다.
- 소비자가 ACK 을 Queue에 전달하지 못하는 경우는 받은 메시지가 너무 커 아직 처리중이거나 소비자 서버가 죽었을 때입니다.
- RabbitMQ 에서는 ACK을 받지 못한 메시지의 경우, 대기를 하고 있다가 전달한 소비자 서버의 상태를 확인한 후, Disconnected 와 같은 신호를 받았을 경우 해당 소비자를 제외하고 다른 소비자에게 동일한 메시지를 전달합니다.

### Message Durability

- 만약 메시지를 Queue 에 넣은 다음 소비자에게 전달하기 전에 RabbitMQ 서버가 죽는다면 Queue는 메모리에 데이터를 쓰는 형식이므로 모든 데이터가 소멸하게 됩니다.
- message durability 는 메시지가 Queue에 저장될 때, disk의 파일에도 동시에 저장하는 방법입니다. 해당 방법을 사용하면 서버가 죽었을 때, Queue의 데이터가 어느정도 복구가 되지만 메시지가 disk의 파일에 쓰는 도중에 서버가 죽는 경우도 있어서 일부 데이터의 소실이 발생할 수 있습니다.

# Reference

---

- [https://www.rabbitmq.com/getstarted.html](https://www.rabbitmq.com/getstarted.html)
- [https://jonnung.dev/rabbitmq/2019/02/06/about-amqp-implementtation-of-rabbitmq/#gsc.tab=0](https://jonnung.dev/rabbitmq/2019/02/06/about-amqp-implementtation-of-rabbitmq/#gsc.tab=0)


