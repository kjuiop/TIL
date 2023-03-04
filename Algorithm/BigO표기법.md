# BigO 표기법

---


## Big O Notation

---

- Big O 표기법의 필요성
    - 수많은 해결법 중 어떤 해결법이 더 좋은지 판단할 수 있을까
    - 코드를 분류하거나 비교할 수 있는 시스템이 있다면 얼마나 좋을까? (등급)
    - 우리의 코드의 수행능력을 얘기할 수 있는 지표
    - 각기 다른 해결법에 대한 장단점 에 대해 얘기할 수 있다.
    - 코드를 debug 할 때 코드를 느리게 만드는 이유를 이해하는 것이 중요하다.

.

## Question) 1부터 N까지 더하는 함수를 만들어라

### ADD1

```java
function addUpTo(n) {
    let total = 0;
    for (let i=1; i<=n; i++) {
    total += i;
    }
    return total;
}

let t1 = performance.now();
addUpTo(1000000000);
let t2 = performance.now();

console.log(`addUpTo Time Elapsed: ${(t2 - t1) / 1000} seconds.`);
```

### ADD2

```java
function addUpTo1(n) {
    return n * (n+1) / 2;
}

let t3 = performance.now();
addUpTo(1000000000);
let t4 = performance.now();

console.log(`addUpTo1 Time Elapsed: ${(t4 - t3) / 1000} seconds.`);
```

- 반복문이 사용되지 않는다.

### 무엇이 더 좋은 코드일까?

- ***속도가 빠른가***
    - 내장되어있는 타이밍 Function 을 사용하는 방법이 있다.
        - `performance.now();`
    - 시간을 빼는 것에는 측정하기 모호한 부분이 있다.
    - 컴퓨터의 사양에 따라 다른 시간이 기록되기도 한다.
    - 빠른 알고리즘에서는 정말 짧은 시간 안에 모든 것이 처리되기 때문에 측정하기 어렵다.
- 메모리를 적게 사용하는가
- 가독성이 좋은가

### 연산 갯수 세기

- 알고리즘의 시간을 측정하는 것이 아닌, 컴퓨터가 처리해야하는 연산 갯수를 센다.
- 알고리즘을 수행하는데 영향을 주는 것은 연산의 갯수이다.

```java
function addUpTo1(n) {
    return n * (n+1) / 2;
}
```

- `*` , `+` , `/` 으로 총 3번의 연산이 이루어진다.
- N 이 크던, 작던 연산은 3번만 이루어진다.

![Screen Shot 2023-03-01 at 12.21.35 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8b71cb17-4cc0-4063-872c-08f14ddd69af/Screen_Shot_2023-03-01_at_12.21.35_PM.png)

<aside>
💡 N이 커질 수록, 연산의 갯수도 비례적으로 늘어난다.

</aside>

### Big O 공식 : 입력의 크기와 실행시간의 관계

---

![Screen Shot 2023-03-01 at 8.14.26 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d97ff326-3f19-4dd5-a473-0ac7ff1f0b05/Screen_Shot_2023-03-01_at_8.14.26_PM.png)

- N이 커질수록 컴퓨터가 f(n) 상수 곱하기 f(n) 보다 간단한 연산을 덜 해야한다면, 그 알고리즘을 O(f(n)) 이라고 표현한다.
    - 선형일 수 있다. N의 값이 커질수록 실행 시간도 같이 늘어난다.

- `O(n)`, `O(n*n)`, `O(nlog n)`, `O(log n)`