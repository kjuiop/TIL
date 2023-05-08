## 요청사항

---

- golang build 를 로컬에서 make 파일로 진행하는데, 이는 로컬의 golang 환경에 따라 버전 이슈가 생기게 됨
- 따라서 Dockerfile 에서 Go bulid version 을 통일하는 것이 필요

```java
FROM golang:1.17 as build

ENV GO111MODULE=on
RUN apt-get update && \
    apt-get install -y build-essential

WORKDIR /usr/src/app
COPY . .
RUN go mod download
RUN make

FROM alpine

MAINTAINER ji.kim <jungin.kim@catenoid.net>

ENV REPORT_LOG_PATH /home/kollus/reporter/logs

RUN apk add curl

RUN mkdir -p /home/kollus/reporter/logs && \
    mkdir -p /home/kollus/reporter/bin

COPY ./reporter /home/kollus/reporter/bin/reporter

WORKDIR /home/kollus/reporter

CMD ["/bin/sh", "-c", "/home/kollus/reporter/bin/reporter"]
```

- 궁금증
    - 만일, COPY . . 일 때 전체 디렉터리를 복사하는 것일까


### ENV GO111MODULE=on

---

- `GO111MODULE` 환경변수는 Go 모듈 지원을 활성화하는데 사용된다.
    - off : 모듈 지원을 비활성화 하고, GOPATH 기반의 전통적인 프로젝트 구조를 사용
    - on : 모듈 지원을 활성화하여 go.mod 파일을 사용하여 의존성 관리를 함
    - auto : Go 버전과 현재 작업디렉토리에 따라 자동으로 지원 활성,비활성

### Dockerfile 의 적절한 위치는?

---

- Dockerfile 의 위치를 Docker 디렉터리가 맞을까, Project 의 Root 경로가 맞을까
    - Dockerfile 은 하나의 프로젝트에서 한 개의 Dockerfile 로 관리
    - ENV 의 경우는 Rancher 나 docker-compose 로 각 분리해서 관리
    - docker build 를 통해 여러 빌드에 필요한 라이브러리들이 필요하기 때문에, go.mod 와 같은 파일의 동일선상에 위치하는 것이 좋다.

### CGO_ENABLED=0

---

- **`CGO_ENABLED=0`**
- go build 할 때 **`CGO_ENABLED=0`** 을 통해 C 언어 코드를 사용하지 않는 것으로 설정
    - 이는 C 언어와의 상호작용을 위한 코드를 사용하지 않는 설정으로, 실행파일의 용량을 줄일 수 있음
- go 이미지를 빌드할 때 ubuntu 버전에 영향을 받는 C 라이브러리 등을 이식하지 않을 수 있음

### GOOS=linux

---

- 컨테이너가 linux를 실행하므로 이 옵션을 설정하면 다른 플랫폼에서 애플리케이션을 빌드할 때도 반복 가능한 빌드를 사용할 수 있음

### ldflags

---

- `ldflags=”-s -w”` 매개변수를 사용하면 빌드 프로세스의 링크단계에서 발생하는 링커 옵션을 사용할 수 있다.
- `-s -w` 옵션은 디버깅 기호의 바이너리를 제거하여 크기를 줄이는 옵션

```java
CGO_ENABLED=0 GOOS=linux go build -ldflags "-X main.BUILD_TIME=`date -u '+%Y-%m-%d_%H:%M:%S'` -X main.GIT_HASH=`git rev-parse HEAD` -X main.BUILD_NUMBER=$(cat build_num.txt) -X main.VERSION_NUMBER=$(cat version.txt) -s -w" -o /usr/src/app/bin/reporter /usr/src/app/main
```

# Reference

---

- [https://www.hahwul.com/2020/10/07/docker-multistage-build-for-optimazation/](https://www.hahwul.com/2020/10/07/docker-multistage-build-for-optimazation/)