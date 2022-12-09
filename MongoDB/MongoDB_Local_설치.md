# MongoDB Local 설치방법

---

### 우분투 버전 확인

```java
lsb_release -dc
```

- 터미널 창에서 위의 명령어를 입력해 Description 부분을 확인하면 현재 설치된 우분투의 버전을 확인할 수 있다.

### 패키지 관리 시스템에서 사용하는 public key 가져오기

```java
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
```

- 터미널 창에서 위의 명령어를 실행하여 MongoDB public GPG key 를 가져온다.

### MongoDB 를 위한 List 파일 만들기

- List 파일을 만드는 방법은 우분투 버전에 따라 다르기 때문에 우분투의 버전에 맞춰 진행해야 한다.

```java
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
```

- 터미널 창에서 위의 명령어를 실행하여 우분투 버전에 맞는 list 파일을 생성해준다.

### 로컬 패키지 데이터베이스 불러오기

```java
sudo apt-get update
```

- MongoDB 패키지 설치를 위해 로컬에 데이터베이스 패키지를 불러온다.

### MongoDB 패키지 설치하기

```java
sudo apt-get install -y mongodb-org
```

- MongoDB 패키지 설치

# MongoDB 실행하기

---

### MongoDB 실행/관리용 init 시스템 확인

- 프로세스를 실행하고 관리하기 위해서 운영체제에 내장된 init 시스템을 활용한다.
- 최신버전의 우분투는 보통 systemd (systemctl) 을 사용하지만, System V Init (service)를 사용하는 경우도 있다.
- 터미널에 다음 명령어를 실행해 사용 중인 init 시스템을 확인할 수 있다.

```java
ps --no-headers -o comm 1
```

- `systemd` - systemd (systemctl) 사용
- `init` -  System V Init (service) 사용

## Unrecognized service

---

- 위처럼 init 을 확인하여 service 명령어를 사용하였지만 `service status mongod` 아래의 메시지가 출력되었다.
    - `unrecognized service`
- 이는 ubuntu 의 init.d 에서 mongod 에 대한 파일이 존재하지 않기 때문에 링크되지 않는 것이다.

### /etc/init.d/mongod

- mongodb 깃헙 페이지에서 mongod 파일에 대한 내용을 가져온다.
    - [https://github.com/mongodb/mongo/blob/master/debian/init.d](https://github.com/mongodb/mongo/blob/master/debian/init.d)
- `sudo vi /etc/init.d/mongod` 명령어를 통해 해당 파일을 생성한다.
- `chmod 777 mongod` 명령어를 통해 생성한 파일에 대한 권한을 부여한다.

그러면 정상적으로 service 명령어를 사용할 수 있게 된다.

### MongoDB 실행하기

```java
sudo service mongod start
```


# Reference

---

- [https://velog.io/@seungsang00/Ubuntu-MongoDB-설치하기-Ubuntu-20.04](https://velog.io/@seungsang00/Ubuntu-MongoDB-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-Ubuntu-20.04)
- [https://jipro.tistory.com/45](https://jipro.tistory.com/45)

