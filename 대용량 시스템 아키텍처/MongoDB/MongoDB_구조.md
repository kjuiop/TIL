# MongoDB 구조

---

![1](https://user-images.githubusercontent.com/41246605/209457638-b2fa8daf-044d-4f1f-baf8-78c2a5bd3225.png)

# MongoDB

---

![2](https://user-images.githubusercontent.com/41246605/209457643-320357c0-7d4d-4c23-b846-9c7777b2219e.png)


### Database

- admin
    - 인증과 권한 부여 역할이다.
    - 일부 관리 작업을 하려면 admin Database 에 대한 접근이 필요하다.
        - server shutdown 명령어
        - User 의 Role 에 대한 작업
- local
    - 모든 mongodb instance 는 local database 를 소유한다.
    - oplog 와 같은 replication 절차에 필요한 정보를 저장한다.
    - startup_log 와 같은 instance 진단 정보를 저장한다.
    - local database 자체는 복제되지 않는다.
- config
    - sharded cluster 에서 각 shard 의 정보를 저장한다.

### Collection

- 동적 스키마를 갖고 있어서 스키마를 수정하려면 필드 값을 추가/수정/삭제 하면 된다.
    - Document 를 삽입하면 자동적으로 Collection 이 생성된다.

        ```java
        db.message.insertOne({
        ... user: "tom",
        ... age: "23",
        ... log: "hello MongoDB!"
        ... })
        ```

        - message Collection 이 없는 경우에도, 위처럼 Document 를 삽입하면 message Collection 이 자동으로 생성된다.

        ```java
        db.message.insertOne({
        ... name: "billy",
        ... age: 23,
        ... log: "MongoDB is Fun!!",
        ... date: "2022-12-25"
        ... })
        ```

        - 위처럼 Field 와 dataType 이 다르더라도 같은 Collection 안에 데이터가 저장된다.

        ```java
        db.message.find()
        ```

      ![3](https://user-images.githubusercontent.com/41246605/209457644-2f81ccf9-9bdd-4b25-b09f-7d263ad7afe7.png)



이것이 가능한 이유는 MongoDB 가 데이터를 저장할 때 bson (binary json) 형태로 저장하기 때문이다.

그러나 아래와 같이 Index 나 Shard 는 Collection 단위로 이루어지기 때문에, 규격에 맞는 Schema 를 갖는 것이 좋다.

- Collection 단위로 Index 를 생성할 수 있다.
- Collection 단위로 Shard 를 나눌 수 있다.

### Document

- JSON 형태로 표현하고 BSON (Binary JSON) 형태로 저장한다.
- 모든 Document 에는 “_id” 필드가 있고, 없이 생성하면 ObjectId 타입의 고유한 값을 저장한다.
- 생성 시, 상위 구조인 Database 나 Collection 이 없다면 먼저 생성하고 Document 를 생성한다.
- Document 의 최대 크기는 **16MB** 이다.
    - Schema 가 동적이기 때문에 Document 의 최대 사이즈가 지정되었다.

### 요약

- Database → Collection → Document → Field 순으로 구조가 형성되어 있다.
- admin, config, local database 는 MongoDB 를 관리하는데 사용된다.
- Collection 은 동적 스키마를 갖는다.
- Document 는 JSON 형태로 표현되고, BSON 형태로 저장된다.
- Document 는 고유한 “_id” 필드를 항상 갖고 있다.
- Document 의 최대 크기는 16MB 로 고정되어있다.

