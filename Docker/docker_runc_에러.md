# failed to retrieve runc version: unknown output format

---

## 상황

- 도커 컨테이너가 이유없이 도중 재시작을 하였다.
- 이에 대한 원인을 찾던 중 syslog1 파일에 아래와 같은 로그가 남아있었다.
- `msg="failed to retrieve runc version: unknown output format: runc version 1.0.0-rc95`

```go
Jan  9 06:18:00 kr01la03 dockerd[1760]: time="2023-01-09T06:18:00.021814288Z" level=warning msg="failed to retrieve runc version: unknown output format: runc version 1.0.0-rc95\ncommit: b9ee9c6314599f1b4a7f497e1f1f856fe433d3b7\nspec: 1.0.2-dev\ngo: go1.13.15\nlibseccomp: 2.5.1\n"
```

<br />

### Docker Runc

- runc 은 컨테이너 실행을 위한 오픈 소스 소프트웨어이다.
- 오류 메시지 `failed to retrieve runc version: unknown output format: runc version 1.0.0-rc95` 가 나타날 수 있는 이유는 다음과 같다.
    - `‘runc’` 이 설치되지 않았거나, 설치된 `‘runc’` 의 버전이 잘못된 경우
        - 이 경우에는 ‘runc’ 을 제대로 설치하거나 올바른 버전으로 업그레이드하면 해결할 수 있음
    - `‘runc’` 바인딩이 잘못된 경우, `‘runc’` 이 실행될 때 의존성을 가지고 있는 파일을 참조할 수 없는 경우에 이 오류가 발생할 수 있다.
        - 이 경우 `‘runc’` 의존성을 화깅ㄴ하고, 이를 제공해주는 경로가 제대로 설정되어 있는지 확인한다.
    - `‘runc’` 이 실행되는 운영체제가 지원하지 않는 경우
        - ‘runc’ 은 Linux 운영체제에서만 실행될 수 있기 때문
        - 다른 운영체제가 실행되고 있는 경우에 `‘runc’` 을 실행할 수 없기 때문에 이 오류가 발생할 수 있음
        - 이 경우에는 `‘runc’` 을 지원하는 운영체제로 전환하거나, `‘runc’` 대신에 지원하는 운영체제에서 실행 가능한 다른 컨테이너 실행 소프트웨어를 사용해 보는 것이 좋다.

## Docker Version

---

- docker version : 18.09.9
    - runc version : 1.0.0-rc95
        - commit : b9ee9c6314599f1b4a7f497e1f1f856fe433d3b7
        - spec : 1.0.2-dev
        - go : go1.13.15
        - libseccomp : 2.5.1


<br />


## 해결방법

---

- `runc` 을 제거하고, 재설치하거나 최신 버전으로 업그레이드한다.

```go
sudo apt-get remove runc
sudo apt-get update
sudo apt-get install runc
```

- `runc` 이 의존하고 있는 파일들이 제대로 설치되어 있는지 확인한다.
    - **`/usr/lib/x86_64-linux-gnu/libapparmor.so.1`**
    - **`/usr/lib/x86_64-linux-gnu/libcgroup.so.1`**
    - **`/usr/lib/x86_64-linux-gnu/libseccomp.so.2`**
    - **`/usr/lib/x86_64-linux-gnu/libsystemd.so.0`**

- `runc` 은 Linux 운영체제에서만 실행될 수 있기 때문에 운영체제가 Linux 인지 확인한다.


