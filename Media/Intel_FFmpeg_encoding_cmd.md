## FFmpeg CMD Option

---

- `-extbrc`
    - Extended bitrate control (from -1 to 1) (default -1)
    - FFmpeg 에서 Intel Quick Sync Video(QSV) 를 사용할 때 적용할 수 있는 옵션
    - QSV 는 Intel 의 하드웨어 장비, 그래픽카드를 사용하여 동영상 인코딩 및 디코딩 처리를 가속화하는 기술
    - `-extbrc` 를 사용하면 인코더가 외부 비트레이트 제어 알고리즘을 사용하도록 설정할 수 있음
    - `-extbrc 1` 옵션은 외부 비트레이트 제어를 활성화한다는 의미
    - 추가 라이브러리나 소프트웨어를 사용한다.
        - Intel Media SDK
            - h.265까지는 Intel Media SDK → Open VPL 로 넘어감
            - 이 SDK 는 Intel 플랫폼에서 멀티미디어처리를 위한 하드웨어 가속 기능을 제공
            - 여기에는 인코딩, 디코딩, 비디오 처리 기능이 포함되어 있음
            - QSV 와 함께 사용할 수 있는 외부 비트레이트 제어 알고리즘을 구현하는 라이브러리를 포함할 수 있습니다.
        - Intel OneAPI Video Processing Library(OneVPL)
            - oneVPL 은 Intel 의 비디오 처리 라이브러리로, 인코딩 및 디코딩, 색상 공간 변환 등의 기능을 제공합니다.
            - 이 라이브러리는 Intel Media SDK 의 후속 버전으로 간주되며, oneVPL 을 함께 사용하면 외부 비트레이트 제어 알고리즘을 사용할 수 있습니다.

- `-look_ahead_depth`
    - Depth of look ahead in number frames, available when extbrc option is enabled (from 0 to 100) (default 0)
    - 인코딩 프로세스에서 look-ahead 알고리즘의 프레임수를 지정합니다.
    - look-ahead 알고리즘은 여러 프레임을 미리 분석하여 비트 할당, B-프레임 결정, 영상의 품질-비트레이트 트레이드 오프 등을 최적화합니다.
    - look-ahead-depth 옵션은 인코더와 코덱에 따라 다르게 적용됩니다.
        - h.264 의 경우에는 rc-lookahead 를 사용합니다.