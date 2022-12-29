# awk 명령어로 Docker 스크립트 파일 생성하기

---

### 02.attach_apig.sh

```jsx
#!/bin/bash

CON_ID=`docker ps | grep project-name | awk '{print$1}'`

docker cp ./test.sh $CON_ID:/home/project/test.sh

docker exec -it -u 0 ${CON_ID} /bin/sh
```

attach_apig.sh 는 Docker 컨테이너에 접속하는 실행파일이다.

여기서 awk 명령어가 생소하기 때문에 간단하게 정리한다.

### AWK 개념

---

AWK 는 텍스트가 저장되어 있는 파일을 원하는대로 필터링하거나 추가해주거나 기타 가공을 통해서 나온 결과를 행과 열로 출력해주는 프로그램입니다.

SQL 과 같이 특정 텍스트로 읽은 문자를 조합해 행과 열로 나타낼 수 있습니다.


![1a](https://user-images.githubusercontent.com/41246605/209905503-2ff4078a-de53-4abf-92a3-a547b20792d1.png)

<br />
AWK 에서는 레코드가 $0 , $1… ,  $N 은 필드를 나타내는 열을 의미합니다.

file : awk_test_file.txt

```jsx
name    phone           birth           sex     score
reakwon 010-1234-1234   1981-01-01      M       100
sim     010-4321-4321   1999-09-09      F       88
nara    010-1010-2020   1993-12-12      M       20
yut     010-2323-2323   1988-10-10      F       59
kim     010-1234-4321   1977-07-17      M       69
nam     010-4321-7890   1996-06-20      M       75
```

```jsx
awk '{ print $1}' ./awk_test_file.txt 
```

위와 같이 특정 텍스트의 1열을 출력할 수 있습니다.

[결과값]

```jsx
name
reakwon
sim
nara
yut
kim
nam
```

### awk 를 활용한 Docker 명령어

---

```jsx
CON_ID=`docker ps | grep project-name | awk '{print$1}'`
```

<aside>
💡 결국 위의 명령어는 docker 현재 가동중인 프로세스 목록을 출력할 때, mongo 라는 Text 를 가진 행만 출력하고, awk 를 통해 1열을 출력해라

</aside>

라는 의미를 내포하고 있습니다.

`Docker ps` 명령어를 사용하면 첫 번째 열은 ContainerID 가 출력되기 때문이죠.

- docker ps 명령어 결과값

```jsx
[Container ID] [IMAGE] [COMMAND]              [CREATED]        [STATUS]    [PORT]
654ca7932155   project  "docker-entrypoint.s…"  2 weeks ago    Up 47 hours  ...
```

<aside>
💡 awk 를 통해 Docker container ID 를 변수로 가져온 후 Docker 명령어를 실행하는 스크립트 파일이었습니다.

</aside>

# Reference

---

- [https://reakwon.tistory.com/163](https://reakwon.tistory.com/163)