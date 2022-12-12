## Ubuntu Local 환경에서 Zookeeper 설치하기

[Apache Downloads](https://www.apache.org/dyn/closer.lua/zookeeper/zookeeper-3.7.1/apache-zookeeper-3.7.1-bin.tar.gz)

- Zookeeper 공식 홈페이지에서 tar.gz 를 받을 수 있는 link 를 가져온다.

```jsx
# 설치 디렉터리
cd /usr/local

# zookeeper tar.gz 가져오기
sudo wget https://www.apache.org/dyn/closer.lua/zookeeper/zookeeper-3.7.1/apache-zookeeper-3.7.1-bin.tar.gz

# tar.gz 압축파일 풀기
sudo tar xvf apache-zookeeper-3.7.1-bin.tar.gz

# 심볼릭 링크 설정
ls -s apache-zookeeper-3.7.1-bin zookeeper

```

- 심볼릭 링크를 설정하는 이유는 만약 주키퍼의 버전이 변경하게 되면 배포 스크립트 등에 설정되어 있는 경로를 매번 변경해야 하기 때문이다.

### 주키퍼 설정

다음으로 znode 의 복사본인 스냅샷과 트랜잭션 로그들이 저장될 별도의 데이터 디렉토리를 생성한다.

- znode 에 변경상항이 발생하면 이러한 변경사항은 트랜잭션 로그에 추가되며, 로그가 어느정도 커지면 현재 모든 지노드의 상태 스냅샷이 파일시스템에 저장된다.
- 중요한 디렉터리이므로 설치경로와는 다른 경로로 설정한다.

```jsx
# data 를 저장할 디렉터리를 생성한다.
mkdir -p /data/zookeeper

# zookeeper 디렉터리에 권한을 부여한다.
chmod 777 /data/zookeeper

# 주키퍼 znode 의 아이디를 myid 라는 파일로 등록해준다.
echo 1 > /data/zookeeper/myid

# 설치된 주키퍼 경로에서 config 파일을 생성해준다.
vi /usr/local/zookeeper/conf/zoo.cfg

```

- zoo.cfg

```jsx
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data/zookeeper
clientPort=2181
server.1=zk01:2888:3888
server.2=zk02:2888:3888
server.3=zk03:2888:3888
```

- 각 항목에 대한 자세한 설명은 [http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_configuration](http://zookeeper.apache.org/doc/current/zookeeperAdmin/html#sc_configuration) 에서 알 수 있음
- ticketTime : 주키퍼가 사용하는 시간에 대한 기본 측정 단위(밀리초)
- initLimit : 팔로워가 리더와 초기에 연결하는 시간에 대한 타임아웃 tick 의 수
- syncLimit : 팔로워가 리더와 동기화 하는 시간에 대한 타임아웃 tick 의 수
    - 주키퍼에 저장된 데이터가 크면 더 크게 잡아야 한다.
- dataDir : 주키퍼의 트랜잭션 로그와 스냅샷이 저장되는 저장경로. 이 글에서는 /data 로 생성했었기 때문에 zoo.cfg 에서도 /data 로 지정
- clientPort : 주키퍼 사용 TCP 포트
- server.x : 주키퍼 앙상블 구성을 위한 서버 설정. server.myid 형식으로 사용
    - 2888:3888 은 기본 포트이며, 앙상블 내 노드끼리 연결 & 리더 선출에 사용
    - 간혹 주키퍼 기동 시 server.x 에 명시된 자기 자신의 호스트명을 인식하지 못하는 경우가 있음. 그럴 경우 자기 자신의 호스트명은 0.0.0.0 으로 변경하면 정상동작함.
        - `server.1=0.0.0.0:2888:3888`
        - [stackoverflow.com/questions/30940981/zookeeper-error-cannot-open-channel-to-x-at-election-address](https://stackoverflow.com/questions/30940981/zookeeper-error-cannot-open-channel-to-x-at-election-address)
- 호스트파일 및 방화벽 설정
    - zoo.cfg 에서 로컬 서버의 호스트명을 제외한 호스트명(예를 들면 zk01 서버에서의 zk02, zk03) 은 인식이 안되므로 zk01, zk02, zk03 서버의 /etc/hosts 파일에 호스트명을 추가해줘야 한다.

```jsx
[zk02 서버의 ip] zk02
[zk03 서버의 ip] zk03
```

- 또한 zoo.cfg 에서 사용한 주키퍼 클라이언트 포트인 2181, 앙상블끼리 연결 & 리더 선출에 사용하는 2888, 3888 포트도 방화벽을 오픈해줘야 한다.


### 주키퍼 실행하기

```jsx
sudo /usr/local/zookeeper/bin/zkServer.sh start
```

- 위의 명령어로 실행하면 zookeeper 가 정상적으로 동작함을 알 수 있다.

```jsx
sudo /usr/local/zookeeper/bin/zkServer.sh stop
```

- 위의 명령어로 zookeeper 를 종료할 수 있다.

### Systemd로 주키퍼 관리하기

```jsx
sudo vi /etc/systemd/system/zookeeper-server.service
```

- zookeeper-server.service

```jsx
[Unit]
Description=zookeeper-server
After=network.target

[Service]
Type=forking
User=root
Group=root
SyslogIdentifier=zookeeper-server
WorkingDirectory=/usr/local/zookeeper
Restart=always
RestartSec=0s
ExecStart=/usr/local/zookeeper/bin/zkServer.sh start
ExecStop=/usr/local/zookeeper/bin/zkServer.sh stop
```

- systemd 의 파일을 만들거나 수정한 후에는 systemd 를 재시작해주어야 한다.

```jsx
systemctl daemon-reload
```

```jsx
# zookeeper server start
systemctl start zookeeper-server

# zookeeper server status
systemctl status zookeeper-server

# zookeeper server stop
systemctl stop zookeeper-server
```

# Reference

---

- [https://twofootdog.tistory.com/89](https://twofootdog.tistory.com/89)

