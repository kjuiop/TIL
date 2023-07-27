# 소나큐브 내용 정리 

---


소나큐브는 크게 Scanner 와 SonarQube Server 로 구분해서 생각하면 쉽습니다.

`homebrew` , `wget` 등 sonarqube scanner 오픈소스를 다운로드 받습니다. 
그리고 이 Scanner 를 통해서 우리의 프로젝트를 분석하고 결과파일을 생성합니다.

해당 결과파일을 HTTP 프로토콜을 통해 SonarQube Server 로 전송합니다. 
SonarQube Server 는 대시보드 형태로 분석 결과를 도표화하여 시각적으로 우리에게 보여줍니다.


# 프로젝트 별 매개변수 적용

---

프로젝트 루트 위치에 [sonar-project.properties](http://sonar-project.properties) 파일로 매개변수를 적용합니다.

### sonar-project.properties

```java
sonar.projectKey=sonar-golang
sonar.projectName=golang-002
sonar.sources=.
sonar.exclusions=**/*_test.go,**/vendor/**,**/testdata/*
sonar.tests=.
sonar.test.inclusions=**/*_test.go
sonar.test.exclusions=**/vendor/**
sonar.go.coverage.reportPaths=./coverage.out
```

- sonar.projectKey : 프로젝트를 식별하기 위한 고유 키
- sonar.projectName : 프로젝트 대시보드에서 명시되는 프로젝트 이름
- sonar.sources : sonnar scanner 가 분석하고자 하는 소스 위치
- sonar.exclusions : 분석에서 제외할 파일
- sonar.tests : 테스트 소스 코드의 경로
- sonar.test.inclusions : 분석에 포함할 테스트 파일 패턴
- sonar.test.exclusions : 분석에서 제외할 테스트 파일 패턴
- sonar.go.coverage.reportPaths : 코드 커버리지 보고서의 경로

위의 properties 에는 우리에게 중요한 sonar server 의 호스트 주소와 토큰에 대한 정보가 빠져있습니다. 이는 소스트리에 올라가지 않게 환경변수로 지정하여 사용할 수 있게 합니다.

### Argument 로 Sonarqube 매개변수 전달

---

```java
sonar-scanner \
  -Dsonar.host.url=http://your_sonarqube_server_url \
  -Dsonar.login=your_sonarqube_token \
  -Dsonar.projectKey=project_key \
  -Dsonar.projectName="Project Name" \
  -Dsonar.projectVersion=1.0 \
  -Dsonar.sources=app,config,resources,tests \
  -Dsonar.sourceEncoding=UTF-8
```

# Sonarqube Dashboard 

---


- Code Smell : 심각한 이슈는 아니지만 베스트 프렉티스에서 사소한 이슈들로 모듈성(modularity), 이해가능성(understandability), 변경가능성(changeability), 테스트용의성(testability), 재사용성(reusability) 등이 포함된다.
- Bugs : 일반적으로 잠재적인 버그 혹은 실행시간에 예상되는 동작을 하지 않는 코드를 나타낸다.
- Vulnerabilities : 해커들에게 잠재적인 약점이 될 수 있는 보안상의 이슈를 말한다. SQL 인젝션, 크로스 사이트 스크립팅과 같은 보안 취약성을 찾아낸다.
- Duplications : 코드 중복은 코드의 품질을 저해시키는 가장 큰 요인 중 하나이다.
- Unit Test : 단위테스트 커버리지를 통해 단위 테스트의 수행 정도와 수행한 테스트의 성공/실패 정보를 제공한다.
- Complexity : 코드의 순환 복잡도, 인지 복잡도를 측정합니다.
- Size : 소스코드 사이즈와 관련된 다양한 지표를 제공합니다.