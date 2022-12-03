## Amazon ECR

---

Amazon Elastic Container Registry (Amazon ECR) 는 이미지 레파지토리를 생성, 모니터링 및 삭제하고, 레파지토리 액세스할 수 있는 사용자를 제어하는 권한을 설정하는 API 작업을 제공합니다.

<br />

## 프라이빗 ECR 레파지토리 생성

---

1. AWS 서비스에서 ECR 을 검색해서 콘솔 창으로 들어옵니다. 이때 리전은 서울로 지정합니다.


![1](https://user-images.githubusercontent.com/41246605/205426845-0b3a13f3-523c-48f5-8ba1-66da828901d5.png)


2. 리포지토리 생성에서 프라이빗을 선택, 이름을 지정합니다.

![2](https://user-images.githubusercontent.com/41246605/205426864-d09b4cf1-20fd-41da-859e-b670fd894ae5.png)


3. 생성된 리포지토리에서 푸시 알림 보기를 클릭하면 AWS CLI 명령어를 안내받을 수 있습니다.

![3](https://user-images.githubusercontent.com/41246605/205426868-14ef2b84-983e-4e59-bcbd-7283ed4a794e.png)


4. Docker build 및 Push 명령어를 아래와 같이 사용합니다.

![4](https://user-images.githubusercontent.com/41246605/205426874-752558ec-7f81-4b52-a62a-273a6166a54f.png)


# Reference

---

- [https://docs.aws.amazon.com/ko_kr/AmazonECR/latest/userguide/Repositories.html](https://docs.aws.amazon.com/ko_kr/AmazonECR/latest/userguide/Repositories.html)

