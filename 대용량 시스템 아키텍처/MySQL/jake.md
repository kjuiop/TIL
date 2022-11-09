
# OpenSource

---

[https://github.com/mysql/mysql-server](https://github.com/mysql/mysql-server)

[MySQL: Welcome](https://dev.mysql.com/doc/dev/mysql-server/latest/)



<br />

### 데이터베이스는 파일을 관리하는 서버

![Screen Shot 2022-11-09 at 11 37 58 AM](https://user-images.githubusercontent.com/41246605/200749453-e33276a7-a51d-4c93-bece-703530b179fd.png)


- 쿼리를 통해 요청을 받아 데이터를 응답하는 서버와 같음
- MySQL 엔진은 두뇌의 역할, 스토리지 엔진은 팔, 다리의 역할

<br />

![Screen Shot 2022-11-09 at 11 38 23 AM](https://user-images.githubusercontent.com/41246605/200749466-5dc4c0d9-fbba-4dd7-9f04-4a8b514297df.png)


### 쿼리 파서

- SQL 을 파싱하여 Syntax Tree 를 만듬
- 이 과정에서 문법 오류 검사가 이루어짐

### 전처리기

- 쿼리파서에서 만든 Tree 를 바탕으로 전처리 시작
- 테이블이나 컬럼 존재 여부, 접근권한 등 Semantic 오류 검사


<br />


![Screen Shot 2022-11-09 at 11 39 53 AM](https://user-images.githubusercontent.com/41246605/200749473-95958e29-ba85-45e3-b7e4-1edcd9d47a8c.png)



- 쿼리파서, 전처리기는 컴파일 과정과 매우 유사하다.
- 하지만 SQL 은 프로그래밍 언어처럼 컴파일 타임 때 검증할 수 없어 매번 구문 평가를 진행

### 옵티마이저

- 쿼리를 처리하기 위한 여러 방법들을 만들고, 각 방법들의 비용정보와 테이블의 통계정보를 이용해 비용을 산정
- 테이블 순서, 불필요한 조건 제거, 통계정보를 바탕으로 전략을 결정 (실행 계획 수립)
- 옵티마이저가 어떤 전략을 결정하느냐에 따라 성능이 많이 달라짐
- `explain` 명령을 통해 볼 수 있음



![Screen Shot 2022-11-09 at 11 41 42 AM](https://user-images.githubusercontent.com/41246605/200749493-93fd9b65-1516-4772-b98e-3f966f545979.png)


- 스토리지 엔진에 요청하는 것이 Handler API


# 쿼리 캐시

---

![Screen Shot 2022-11-09 at 11 42 20 AM](https://user-images.githubusercontent.com/41246605/200749496-9e3a60ed-efa5-41b2-98f0-e412a2c7d25f.png)


- MySQL 5.0 까지는 쿼리 캐시라는 것이 있었으나 8.0 대에는 쿼리캐시가 폐기됨
    - 데이터를 캐싱하고 있을 때, 데이터가 변경되면 람다 이슈가 발생했었음

- 소프트 파싱 : SQL, 실행계획을 캐시에서 찾아 옵티마이저 과정을 생략하고 실행 단계로 넘어감
- 하드 파싱 : SQL, 실행계획을 캐시에서 찾지못해 옵티마이저 과정을 거치고나서 실행단계로 넘어감

### MySql

- MySQL 에는 소프트 파싱이 없다.
- 5버전까지는 쿼리캐시가 있었으나 8버전 이후에는 사라짐
- 쿼리캐시는 SQL에 해당하는 데이터를 저장하는 것
- 쿼리캐시는 데이터를 캐시하기 때문에 테이블의 데이터가 변경되면 캐시의 데이터도 함께 갱신시켜줘야 함

### Oracle

- Oracle 에는 소프트 파싱이 존재
- 실행계획까지만 캐싱
- 하지만 모든 SQL 과 맵핑해 데이터까지 캐싱하지는 않음 (힌트나 설정으로 가능)

### MySQL 과 Oracle 의 차이

- MySQL 의 쿼리캐시, Oracle 의 소프트 파싱 모두 성능 최적화를 위해 캐시라는 기술을 도입하였음
    - 그러나 캐시의 범위가 다르다
    - 캐시를 도입할 때는 항상 만료정책을 고려해야함
    - 쿼리캐시는 소프트 파싱에 비해 조회성능은 더 높지만 캐시 데이터 관리에 더 높은 비용이 들어감


<br />

## 스토리지 엔진

---

![Screen Shot 2022-11-09 at 11 46 59 AM](https://user-images.githubusercontent.com/41246605/200749501-8bf4ce0e-e8bd-4caa-af34-c38376574f1c.png)


- 디스크에서 데이터를 가져오거나 저장하는 역할
- MySQL 스토리지 엔진은 플러그인 형태로 Handler API 만 맞춘다면 직접 구현해서 사용 가능
- InnoDB, MyIsam 등 여러개의 스토리지 엔진이 존재
- 8.0 대부터는 InnoDB 엔진을 디폴트

## Keyword

---

- Clustered Index
- Redo - Undo
- Buffer pool



