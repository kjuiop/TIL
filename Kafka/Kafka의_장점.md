- Kafka 의 장점

    - High throug`hput message capacity
        - Kafka 는 고 가용성으로, Producer (Websocket을 받는 Chatting-Server) 가 메시지를 발행하는 순간 Partition 에 메시지를 보관합니다.
        - 분산처리가 가능하기 때문에 짧은 시간 내에 엄청나게 많은 데이터를 Consumer 에게 전달할 수 있습니다.
        - 짧은 시간 내에 많은 데이터를 Consumer 에게 전달할 수 있습니다.
  
    - Scalability 와 Fault tolerant
        - 카프카는 확장성이 뛰어나므로, 브로커 중 몇대가 죽더라도 Replica 로 복제된 데이터로 안전하게 보관할 수 있습니다.
        - 가령, Websocket 으로 연결된 채팅 서버 중 1대가 죽거나 Restart 가 된다 하더라도, 해당 채팅메시지는 Kafka 의 Topic 에 보관되어 있기 때문에 재시작 후 다시 채팅 메시지를 받을 수 있습니다.
        - 기존, 연결이 끊어졌을 때 채팅메시지를 못 받았던 부분을 kafka 를 이용한다면 client 에서 다시 Reconnect 하는 것만으로 이전 채팅 메시지들을 다시 받을 수 있습니다.
  
    - Undeleted log
        - 컨슈머의 그룹 아이디만 다르게 하면 동일한 데이터도 각각 다르게 처리 가능합니다.
        - 현재 kollus-live-chat 은 Log 기반으로 Logstash 가 로그를 수집하여 통계처리를 하고 있으나, Kafka 로 채팅 메시지를 관리할 때, 직접적으로도 수집 가능합니다.
        - 동일한 데이터를 채팅서버 메시지 전달용, 통계용, 등등 다양한 데이터파이프 라인으로 활용할 수 있습니다.