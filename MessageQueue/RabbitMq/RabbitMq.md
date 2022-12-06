# RabbitMq 란

---

## RabbitMQ 동작 이해하기

비동기 작업 큐 (예: Celery) 를 사용하려면 중간 단계에 Broker 라고 부르는 메시지 큐가 항상 등장한다.
메시지 큐에는 RabbitMQ, ActiveMQ, ZeroMQ, Kafaka 등이 대표적이다.

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

### Exchange의 종류

- Direct
    - 직접 라우팅 키와 일치하는 큐에 메시지를 전달하는 방식
- Fanout
    - 모든 메시지 큐에 publish 하는 방식
- Topic
    - Topic은 .으로 구분된 라우팅 키중에 특정 위치에 속하는 그룹에만 보내는 방식
- Headers
    - Headers는 x-match키와 밸류 any나 all과 같은 논리식을 통해 헤더를 가지고 있는 exchange인지 아닌지 판단하여 보내는 방식

<br />

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