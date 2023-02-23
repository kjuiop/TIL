# NVIDIA VideoSDK 12.0 

---


### 하드웨어 기반 디코더 및 인코더

- 공식 문서
    - [https://docs.nvidia.com/video-codec-sdk/12.0/index.html](https://docs.nvidia.com/video-codec-sdk/12.0/index.html)
- NVIDIA 의 최신 GPU 아키텍처 Ada 는 최대 3배의 NVENC 를 제공합니다.
- 비디오 코덱 SDK 12.0 은 여러 인코더에 부하를 고르게 분산하여 AV1 및 HEVC 형식에 대한 실시간 8k60 인코딩을 지원합니다.


### CUDA Toolkit Download Link

- 공식문서
  - [https://developer.nvidia.com/cuda-toolkit](https://developer.nvidia.com/cuda-toolkit)
- NVIDIA CUDA Toolkit 은 고성능 GPU 가속 애플리케이션을 만들기 위한 개발 환경을 제공합니다.
- 이 툴킷에는 애플리케이션을 배포하기 위한 GPU 가속 라이브러리, 디버깅 및 최적화 도구, C/C++ 컴파일러 및 런타임 라이브러리가 포함되어 있습니다.
- 예시) - 운영체제 별 다운로드 링크를 알려줌



### Compiling For Linux

---

- clone ffnvcodec

```java
git clone https://git.videolan.org/git/ffmpeg/nv-codec-headers.git
```

- install ffnvcodec

```java
cd nv-codec-headers && sudo make install && cd –
```

- clone FFmpeg’s publid GIT Repository

```java
git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg/
```

- install necessary packages

```java
sudo apt-get install build-essential yasm cmake libtool libc6 libc6-dev unzip wget libnuma1 libnuma-dev
```

- configure

```java
./configure --enable-nonfree --enable-cuda-nvcc --enable-libnpp --extra-cflags=-I/usr/local/cuda/include --extra-ldflags=-L/usr/local/cuda/lib64 --disable-static --enable-shared
```

- compile

```java
make -j 8
```

- install the libraries

```java
sudo make install
```

### NVENC - 하드웨어 가속 비디오 인코딩

- NVIDIA 의 Ada 아키텍처에서 비디오 코덱 SDK 12.0 을 사용한 AV1 인코딩을 소개합니다.
- AV1 은 H.264 및 HEVC 에 비해 더 나은 성능으로 더 높은 품질을 지원하는 최첨단 비디오 코덱 형식이빈다.



