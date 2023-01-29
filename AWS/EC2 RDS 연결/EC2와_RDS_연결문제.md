### 문제 상황

1. EC2 RDS 보안그룹 인/아웃바운드 규칙으로 연결이 잘 되어 있다.
2. 로컬 상에서는 RDS 연결이 잘 안된다.
3. EC2 서버에서는 연결이 안된다.

### Error Code

1. java.net.UnknownHostException: [main-project.ctdssl3yscyh.ap-northeast-2.rds.amazonaws.com](http://main-project.ctdssl3yscyh.ap-northeast-2.rds.amazonaws.com/): Name or service not known
2. com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure
3. The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.

위의 문제상황을 보았을 때, EC2 서버와 RDS 서버와의 통신이 안되는 것으로 보이네요.

먼저 EC2 서버에서 RDS Host 와의 커넥션 자체를 확인해봐야겠습니다.

- dig 명령어 (linux) 나 nslookup (window) 를 통해 RDS 엔드포인트와의 통신이 이루어지는지 1차적 확인이 필요하겠습니다.

이때 통신이 이루어지지 않는다면 저희는 2가지를 확인해볼 수 있겠네요

- EC2 와 RDS 의 보안그룹 구성이 올바른지 (EC2 인스턴스와 RDS DB 인스턴스는 같은 Security Group 에 속하는지)
- 2개의 서비스가 같은 VPC (같은 망) 에 존재하는지
    - RDS 와 EC2 가 같은 Region 에 있는지
    - RDS DB 인스턴스를 접속할 때 Endpoint 를 사용하였는지
- 배포 IAM 계정의 권한이 적절한지
    - 배포 권한을 가지고 있는 IAM 계정이 배포 하고자 하는 서비스에 대한 권한을 올바르게 갖고있는지
        - EC2, RDS, etc …

- 우선적으로, RDS 와 EC2 간 VPC, Region, Security Group 이 공유되는지 체크해보시고,
- 두번째로, 모든 권한을 가진 IAM 계정이 배포를 진행했을 때와 배포용 권한만 가진 IAM 계정으로 배포했을 때의 배포 성공여부를 체크해보시면 좋을 것 같습니다.

---

현재는 Code Deploy 로 배포했을 때의 Deploy Logging 으로 문제파악이 어렵다면,

프로젝트의 Jar 파일을 EC2 서버에 수동 배포를 진행해보면 Logging 을 바로바로 볼 수 있을 것입니다.

만일 Mac 을 사용하신다면 SCP 명령어를 통해 Jar 파일을 업로드할 수 있고, Window 를 사용하신다면 파일질라 애플리케이션을 통해 Jar 파일을 업로드할 수 있겠네요.