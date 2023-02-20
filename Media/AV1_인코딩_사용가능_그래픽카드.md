
💡 엔비디아 계열 그래픽카드가 AV1 코덱을 지원하는지 ?


- NVIDIA 는 최신 그래픽 카드에서 AV1 코덱 디코딩 지원을 제공합니다.
- NViDIA 는 GeForce RTX 30 시리즈 및 이후의 그래픽 카드에서 AV1 디코더를 지원합니다.
- AV1 코덱의 인코딩은 그래픽 카드가 아닌 CPU 에서 처리되기 때문에 인코딩을 수행하려는 경우에는 엔비디아의 그래픽 카드는 AV1 인코딩 지원을 제공하지 않습니다.

💡 엔비디아 AV1 코덱 지원 사양

- NVIDIA GeForce 1050 Ti 이상 또는 AMD Radeon RX 470 이상
- 최소 4GB 이상의 그래픽 메모리
- 최소 4GB 이상의 시스템 메모리
- DirectX 11 이상 또는 OpenGL 4.3 이상을 지원하는 그래픽 카드


💡 AV1 인코딩을 지원하기 위한 CPU 사양

- AV1 코덱은 현재 인코딩에 매우 높은 컴퓨팅 요구사항을 가지고 있습니다.
    - Intel Core i7/i9 또는 AMD Ryzen 7/9 시리즈 포로세서 이상
    - 최소 8 코어 / 16 스레드 이상
    - 최소 16 GB 이상의 시스템 메모리
    - 최신 버전의 인코딩 소프트웨어
        - x264, x265, FFmpeg
        - 최신 버전의 AV1 인코딩 라이브러리
            - libaom
            

💡 FFmpeg 은 AV1 코덱의 인코딩 및 디코딩 지원 가능 여부

- FFmpeg 을 사용하면 AV1 코덱의 인코딩 및 디코딩을 수행할 수 있음
- AV1 디코딩은 FFmpeg 라이브러리에서 기본적으로 지원됩니다.
    - `ffmpeg -i input.avi output.mp4`
- AV1 인코딩을 위해서는 FFmpeg 을 AV1 인코딩 라이브러리와 함께 빌드해야 합니다.
- FFmpeg 은 libaom 을 포함하여 다양한 AV1 인코딩 라이브러리를 지원합니다.
- FFmpeg 을 사용하여 AV1 코덱의 다양한 옵션을 지정할 수 있으며, 이를 통해 AV1 비디오의 화질, 해상도, 비트레이트 등을 조정할 수 있습니다.
    - `ffmpeg -i input.mp4 -c:v libaom-av1 -crf 30 output.av1`
    - 위 명령어에서 `-c:v libaom-av1` 은 libaom 라이브러리를 사용하여 AV1 코덱을 선택
    - `-crf 30` 은 비디오의 압축률을 제어합니다. 이를 통해 비디오의 화질과 파일 크기를 조정합니다.


💡 libaom 라이브러리와 ffmpeg 과 함께 빌드하는 방법


- libaom 을 다운로드하고 빌드합니다.
    - libaom 최신 버전은 AOMedia 웹 사이트에서 다운로드 받을 수 있습니다.
        - [https://aomedia.org/av1-features/get-started/](https://aomedia.org/av1-features/get-started/)
    - Github 명령어를 사용하여 libaom 을 빌드합니다.

    ```java
    git clone https://aomedia.googlesource.com/aom
    cd aom
    mkdir build
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local
    make
    sudo make install
    ```

    - `cmake` 명령어를 사용하여 libaom 의 빌드를 설정합니다.
    - `-DCMAKE_INSTALL_PREFIX` 옵션을 사용하여 빌드된 파일을 설치할 디렉토리를 지정합니다.
    - 위의 예제에서는 `/usr/local` 디렉터리를 사용합니다.

- FFmpeg 을 다운로드하고 빌드합니다.

    ```java
    git clone https://github.com/FFmpeg/FFmpeg.git
    cd FFmpeg
    ./configure --enable-libaom
    make
    sudo make install
    ```

    - `./configure` 명령어를 사용하여 FFmpeg 의 빌드를 설정합니다.
    - `--enable-libaom` 옵션을 사용하여 libaom 을 FFmpeg 과 함께 빌드할 수 있습니다.
    - 그러면 FFmpeg 에서 AV1 코덱을 사용할 수 있게 됩니다.
    - `make` 명령어를 사용하여 FFmpeg 을 빌드하고, `sudo make install` 명령어를 사용하여 FFmpeg 을 설치합니다.

### Libaom

- libaom 은 AV1 비디오 코덱을 개발하기 위해 Google 에서 개발한 오픈 소스 라이브러리입니다.
- libaom 은 AV1 코덱의 인코딩 및 디코딩을 위한 라이브러리로 C 언어로 작성되어 있으며 BSD 라이선스로 배포됩니다.


💡 AV1 코덱의 인코딩은 어디에서 동작하는가

- AV1 코덱은 CPU 와 GPU 모두에서 실행될 수 있으나, CPU 인코딩은 보다 정교한 제어가 가능하고, GPU 인코딩은 보다 빠른 인코딩 속도를 제공합니다.


💡 CPU 와 GPU 의 차이에 대해 설명해줘

- CPU 와 GPU 는 모두 컴퓨터 중앙 처리 장치 (Central Processing Unit) 입니다.
- CPU
    - CPU 는 다양한 작업에 사용되는 범용 처리 장치입니다.
    - 시스템의 주요 기능인 윤영 체제, 응용 프로그램, 네트워크 등을 관리합니다.
    - CPU 는 단일 코어 또는 다중 코어로 이루어져 있으며, 주로 시스템의 전반적인 성능을 향상시키는 일반적인 계산 작업에 사용됩니다.
    - 또한 CPU 는 캐시 및 RAM 과 같은 기본 메모리에 대한 액세스도 제어합니다.
- GPU
    - GPU 는 그래픽 처리 장치 (Graphics Processing Unit) 으로, 대부분의 시스템에서 이미지와 비디오 처리에 사용됩니다.
    - GPU 는 대규모 데이터 세트를 병렬로 처리할 수 있도록 설계되어 있으며, 이러한 작업은 비디오 인코딩 등에 사용됩니다.
    - GPU 는 수천 개의 작은 코어로 이루어져 있으며, 병렬 처리를 수행하는데 최적화 되어 있습니다.
    - GPU 는 메모리 대역폭이 높은 GDDR 및 HBM 등의 고급 그래픽 메모리를 사용하여 빠른 작업 속도를 제공합니다.

- CPU 는 범용적인 작업을 수행하는 반면, GPU 는 그래픽 처리와 병렬 작업에 최적화되어 있스빈다.


💡 그래픽 카드와 GPU 의 차이


- 그래픽 카드 (Graphics card)
    - 그래픽 카드는 컴퓨터의 주요 구성 요소 중 하나로, 모니터에 그래픽이나 비디오 신호를 출력하는데 사용
    - 그래픽 카등는 CPU 에서 그래픽 처리 작업을 분리하고 별도의 전용 하드웨어를 사용하여 더 빠르고 효율적인 그래픽 처리를 제공합니다.
    - 그래픽 카드에는 그래픽 프로세서, 그래픽 메모리, 출력 포트 등이 있습니다.
- GPU
    - 컴퓨터에서 그래픽 처리를 수행하는 전용 하드웨어 입니다.
    - GPU 는 대개 병렬 처리를 수행하는 수천 개이 작은 코어로 이루어져 있으며, 대량의 데이터를 빠르게 처리할 수 있도록 설계되었습니다.
    - GPU 는 모니터 출력을 담당하는 그래픽 카드에 포함되어 있을 수도 있고, 시스템의 CPU 에 내장되어 있을 수도 있습니다.

- 그래픽 카드는 모니터 출력을 담당하는 장치, GPU 는 병렬 처리를 수행하는 전용 하드웨어 입니다.
- 일부 그래픽 카드 내부에는 매우 강력한 GPU 를 포함하고 있습니다.

### 2021년 기준 NVIDIA 제품 중 AV1 인코딩을 지원하는 제품 (그래픽카드)

- ***NVIDIA Ampere 기반 GPU : (GeForce RTX 30 Series, NVIDIA A100)***
    - Ampre 아키텍처를 사용하는 그래프 카드는 AV1 인코딩에서 특히 높은 성능을 보임
    - NVIDIA 그래픽 카드 중에서 RTX 3080 Ti, RTX 3080, RTX 3070 Ti, RTX 3060 등이 AV1 인코딩을 지원
    - 그 중 RTX 3080 Ti 는 가장 높은 성능을 보이며, 가장 많은 CUDA 코어를 탑재하고 있어 AV1 인코딩 작업에서 가장 높은 처리속도를 제공
- NVIDIA Turing 기반 GPU : (Geforce GTX 16/1660 Supser Series, GeForce RTX 20/2060 Super Series, Quadro RTX 4000/5000/6000)
- NVIDIA Volta 기반 GPU (Titan V, Tesla V100)

### NVIDIA AV1 인코딩을 위한 소프트웨어, 드라이버

- NVIDIA GPU 드라이버 : AV1 인코딩을 지원하는 NVIDIA GPU 를 사용하기 위해서는 GPU용 드라이버가 필요
- FFmpeg : FFmpeg 은 NVIDIA GPU 를 사용하여 비디오 인코딩을 수행할 수 있는 오픈소스 라이브러리
    - AV1 인코딩을 위해서는 FFmpeg 4.3 버전 이상
- libaom : libaom 은 AV1 비디오 코덱의 오픈소스 구현체
    - AV1 인코딩을 수행하기 위해서는 FFmpeg 을 NVIDIA GPU 와 함께 사용하여 libaom 을 빌드해야 함
- CUDA Toolkit : CUDA Toolkit 은 NVIDIA GPU 를 사용한 고성능 컴퓨팅을 위한 소프트웨어 개발 툴킷 (Optional)
    - AV1 인코딩을 수행하는 경우 CUDA Toolkit 을 사용하여 최적화된 성능을 얻을 수 있음
    - CUDA Toolkit 을 사용하면 GPU 의 병렬 처리능력을 최대한 활용하여 더욱 빠르게 인코딩 할수 있으며, 인코딩 프로세스를 더욱 세부적으로 제어할 수 있습니다.

### Ampere 는 NVIDIA 의 최신 GPU 아키텍처 중 하나입니다.

- 이전 Volta 와 Truing 아키텍처에서 기반된 다양한 기술적 개선이 수행되었습니다.
- Ampere 는 특히 AI 및 고성능 컴퓨팅 작업을 위한 설계를 갖오하며, 이를 위해 Tensor Core 연산의 성능이 향상되었습니다.
- Ampere 아키텍처는 새로운 메모리 아키텍처인 NVIDIA 의 2세대 GDDR6X 메모리를 지원하며, PCle Gen 4 인터페이스를 지원하므로 데이터 전송 속도가 크게 향상되었습니다.
- RTX IO 기술을 사용하여 게임 로딩 속도가 대폭 개선ㅇ되었습니다.

### 하드웨어 기반 디코더 및 인코더

- NVIDIA 의 최신 GPU 아키텍처인 Ada 는 최대 3배의 NVENC 를 제공합니다.
- 비디오 코덱 SDK 12.0 은 여러 인코더에 부하를 고르게 분산하여 AV1 및 HEVC 형식에 대한 실시간 8k60 인코딩을 지원합니다.

### 비디오 코드 SDK 12.0

- AV1 은 H.264 및 HEVC 에 비해 더 나은 성능으로 더 높은 품질을 지원하는 최첨단 비디오 코딩 형식입니다.
- Ada 에서 AV1 과 결합된 여러 NVENC 를 사용하면 더 많은 수의 동시 세션과 함께 60fps 에서 8k 비디오를 인코딩할 수 있습니다.
- 완전한 인코딩(계산적으로 복잡)을 NVENC 로 오프로드하면 그래픽 엔진과 CPU 가 다른 작업에 사용됩니다.
    - 예를 들어 OBS 를 사용하여 [Twitch.tv](http://Twitch.tv) 로 스트리밍하는 것과 같은 시나리오에서 인코딩이 NVENC로 완전히 오프로드되면 게임 랜더링에 그래픽 엔진 대역폭을 완전히 사용할 수 있습니다.
- NVENC 는 CPU 를 사용하지 않고 고품질 및 초저지연으로 애플리케이션을 스트리밍하고, 보관을 위해 초고화질로 인코딩하고, OTT 스트리밍, 웹 비디오 및 스트림 당 초저전력 소비(와트/스트림)로 인코딩할 수 있습니다.

# Reference

---

- [https://wikidocs.net/78484](https://wikidocs.net/78484)
- [https://hwtips.tistory.com/3200](https://hwtips.tistory.com/3200)
- [AV1 Encoding and Optical Flow: Video Performance Boosts and Higher Fidelity on the NVIDIA Ada Architecture | NVIDIA Technical Blog](https://developer.nvidia.com/blog/av1-encoding-and-fruc-video-performance-boosts-and-higher-fidelity-on-the-nvidia-ada-architecture/)
- [NVIDIA Video Codec SDK](https://developer.nvidia.com/nvidia-video-codec-sdk)