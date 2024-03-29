# Pub / Sub 구조

---


- Pub/Sub (발행/구독) 메시지 기반 아키텍처에서 자주 사용되는 패턴 중 하나입니다.


- 이 패턴은 발행자 (Publisher) 가 메시지를 발행하고 이를 구독자 (Subscriber) 가 구독하는 방식으로 동작합니다.


- Pub/Sub 구조에서는 발행자는 메시지를 발행하고, 이를 받기 위해 미리 구독한 구독자에게 메시지를 전송합니다. 구독자는 이를 수신하고 필요한 작업을 수행할 수 있습니다.


- 이 패턴은 일반적으로 비동기식으로 동작하며, 발행자와 구독자는 서로를 모르고 직접적인 상호작용을 하지 않습니다.


- Pub/Sub 패턴은 분산 시스템에서 많이 사용됩니다. 여러 서비스가 존재할 경우, 하나의 서비스에서 발행한 메시지를 다른 서비스에서 구독하면서 분산 시스템에서의 통신과 데이터 동기화를 할 수 있습니다.


- Pub/Sub 구조는 대규모 분산시스템에서 매우 유용합니다. 이 패턴은 스케일아웃을 통해 시스템의 용량을 늘리는데 도움이 되며, 메시지 큐를 사용하여 메시지의 지속성을 보장할 수도 있습니다.


- 또한 Pub/Sub 패턴은 마이크로서비스 아키텍처에서도 매우 유용하게 사용되고 있습니다.