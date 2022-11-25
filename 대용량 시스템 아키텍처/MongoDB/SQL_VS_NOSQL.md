# 관계형 데이터베이스

---

- 장점
    - 데이터 중복을 방지할 수 있다.
    - Join 의 성능이 좋다.
    - 복잡하고 다양한 쿼리가 가능하다.
    - 잘못된 입력을 방지할 수 있다.
- 단점
    - 하나의 레코드를 확인하기 위해 여러 테이블을 Join 하여 가시성이 떨어진다.
    - 스키마가 엄격해서 변경에 대한 공수가 크다.

### 관계형 데이터베이스의 Scaling 이 어려운 이유

![1](https://user-images.githubusercontent.com/41246605/203811675-4cc0fcee-65e2-4fd9-918d-c771365569df.png)

- Scale-Out 이 가능하지만, 설정이 어렵다
- 확장할 때마다 App 단의 수정이 필요하다.
- 전통적으로 Scale-Up 위주로 확장했다.


# NoSQL

---


![2](https://user-images.githubusercontent.com/41246605/203811904-0f2a53ec-a7a4-4042-86a7-1062a6c439ee.png)


![3](https://user-images.githubusercontent.com/41246605/203811919-80ff27a8-a437-4c19-8a67-3d73e4fe4295.png)


- 장점
  - 데이터 접근성과 가시성이 좋다.
  - Join 없이 조회가 가능해서 응답 속도가 일반적으로 빠르다.
  - 스키마 변경에 공수가 적다.
  - 스키마가 유연해서 데이터 모델을 App 의 요구사항에 맞게 데이터를 수용할 수 있다.
- 단점
  - 데이터의 중복이 발생한다.
  - 스키마가 자유롭지만, 스키마 설계를 잘해야 성능 저하를 피할 수 있다.

### NoSQL 의 Scaling


![4](https://user-images.githubusercontent.com/41246605/203811929-714d49b6-271a-4980-90cc-41cdb8403265.png)


- HA 와 Sharding 에 대한 솔루션을 자체적으로 지우너하고 있어 Scale-Out 이 간편하다.
- 확장 시, Application 의 변경사항이 없다.

### 요약

- MongoDB 는 Document 지향 Database 이다.
- 데이터 중복이 발생할 수 있지만, 접근성과 가시성이 좋다.
- 스키마 설계가 어렵지만, 스키마가 유연해서 Application 의 요구사항에 맞게 데이터를 수용할 수 있다.
- 분산에 대한 솔루션을 자체적으로 지우너해서 Scale-Out 이 쉽다.
- 확장 시, Application 을 변경하지 않아도 된다.

