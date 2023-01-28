# WebRTC란

- WebRTC 는 P2P (Peer-to-Peer) 통신 기술을 사용하여 브라우저 간에 오디오와 비디오를 전송하며, 이를 위해 특정 프로토콜 (WebRTC MediaStream) 을 사용합니다.
- WebRTC 는 실시간 오디오와 비디오 통신 기능을 제공하므로, 브라우저 간 실시간 화상 통화, 온라인 미팅, 실시간 스트리밍 등의 사용 사례에 적합합니다.

### WebRTC 통신원리

![c1](https://user-images.githubusercontent.com/41246605/214800674-7db8a445-632a-4be0-ad68-0fb4db24331c.png)

- WebRTC 기술은 P2P 통신에 최적화가 되어 있습니다.
- WebRTC에 사용되는 기술은 크게 3가지의 클래스에 의해서 실시간 데이터 교환이 일어납니다.
    - MediaStream : 카메라와 마이크 등의 데이터 스트림 접근
    - RTCPeerConnection : 암호화 및 대역폭 관리 및 오디오, 비디오의 연결
    - RTCDataChannel : 일반적인 데이터의 P2P 통신
- 이 3가지의 객체를 통해서 데이터 교환이 이루어지며, RTCPeerConnection 들이 적절하게 데이터를 교환할 수 있게 처리하는 고자어을 시그널링 (Signaling) 이라고 합니다.
- 위의 그림은 시그널링하는 과정을 나타내며, PeerConnection 은 두명의 유저가 스트림을 주고 받는 것으로 연결을 요청한느 콜러 (Caller) , 연결을 받는 콜리 (Callee) 가 존재합니다.

# Reference

---

- https://medium.com/@hyun.sang/webrtc-webrtc란-43df68cbe511