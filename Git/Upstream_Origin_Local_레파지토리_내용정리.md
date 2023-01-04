
# Upstream, Origin, Local Repository

---

![1a](https://user-images.githubusercontent.com/41246605/210474168-09a8a9c6-beba-41bf-8147-4f4b1002f068.png)

- 개인 Github 계정에서 Upstream Repository 를 Folk 한다.
- 그러면 `관리자계정/프로젝트명` 과 별개로 `내계정/프로젝트명` 이 나의 깃허브에 생성된다.
    - 이때 전자가 `Upstream repository` , 후자가 `Origin Repository` 이다.
- 그 후 Local 에서 `내계정/프로젝트명` 을 Clone 하여 연동할 수 있다.
    - 이때 연결된 프로젝트를 `Local repository` 라 한다.

### Local 에서 Upstream repository 를 등록

```java
# upstream repository 등록
git remote add upstream https://github.com/{관리자계정}/[Project].git

# 등록여부 확인
git remote
> origin
> upstream
```

### Work Flow 전략

- Local Repository 에서 작업을 완료한 후 작업 브랜치를 Origin Repository 에 Push 합니다.
- Github 에서 Origin Repository 에 push 한 브랜치를 Upstream Repository 의 dev 브랜치로 merge 하는 Pull Request 를 생성합니다.
- Pull Request 를 통해 코드 리뷰를 진행한 후 merge 합니다.
- 다시 새로운 작업을 할 때 Local Repository 에서 Upstream Repository 를 pull 합니다.

### Git Convention

![2a](https://user-images.githubusercontent.com/41246605/210474259-4d4bb1a3-c272-4943-b89f-7d8ee8864dc9.png)


# Reference

---

- [https://bioinfoblog.tistory.com/entry/GitHub-개발을-위한-Branch-관리-Upstream-Origin-Local-repository](https://bioinfoblog.tistory.com/entry/GitHub-%EA%B0%9C%EB%B0%9C%EC%9D%84-%EC%9C%84%ED%95%9C-Branch-%EA%B4%80%EB%A6%AC-Upstream-Origin-Local-repository)

