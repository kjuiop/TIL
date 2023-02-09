
# Redis 데이터 타입 (Collection)

---

- Redis 의 장점 중 하나는 Key-Value 스토리지에서 Value 는 단순한 Object 가 아니라 다양한 자료구조를 갖기 때문이다.
- String, Set, Sorted Set, Hash, List 등 다양한 타입을 지원한다.
- Redis Collections 를 사용할 때 주의점
    - 하나의 컬렉션에 너무 많은 아이템을 담으면 좋지 않다.
    - 가능하면 10,000 개 이하의, 몇 천개 수준의 데이터 셋을 유지하는게 Redis 성능에 영향을 주지 않는다.


![z1](https://user-images.githubusercontent.com/41246605/217189352-bfe432ca-0cf9-4ab4-9f80-9d3fe2f7b64a.png)

<br />

# Sorted Sets

---

- set 에 score 라는 필드가 추가된 데이터 형 (score 는 일종의 가중치)
- 일반적으로 set 은 정렬되어있지 않고, insert 한 순서대로 들어간다.
- 그러나 Sorted Set 은 Set 의 특성을 그대로 가지며 추가적으로 저장된 member 들의 순서도 관리한다.
- 데이터가 저장될때부터 score 순으로 정렬되며 저장된다.
- sorted set 에서 데이터는 오름차순으로 내부 정렬된다.
- value 는 중복 불가능, score 는 중복이 가능하다.
- 만약 score 값이 같으면 사전 순으로 정렬되어 저장

<br />

![z2](https://user-images.githubusercontent.com/41246605/217189362-93c7710e-00fe-4be6-b04c-81b95f55ef03.png)


<br />


## Sorted Sets 명령어

---

- SET : ZADD
- GET : ZRANGE, ZRANGEBYSCORE, ZRANGEBYLEX, ZREVRANGE, ZREVRANGEBYSCORE, ZREVRANGEBYLEX, ZRANK, ZREVRANK, ZSCORE, ZCARD, ZCOUNT, ZLEXCOUNT, ZSCAN
- POP : ZPOPMIN, ZPOPMAX
- REM : ZREM, ZREMRANGEBYRANK, ZREMRANGEBYSCORE, ZREMRANGEBYLEX
- INCR : ZINCRBY
- 집합연산 : ZUNIONSTORE, ZINTERSTORE
- Enterprise : ZISMEMBER, ZLS, ZRM, SLEN, SADDS (subquery)



<br />

```java
# ZADD : key에 score-member를 추가
127.0.0.1:6379> zadd fruit 2 apple
(integer) 1
 
127.0.0.1:6379> zadd fruit 10 banana
(integer) 1
 
# 복수개의 score-member를 추가할 수 있음
127.0.0.1:6379> zadd fruit 8 melon 4 orange 6 watermelon
(integer) 3
 
# 이미 추가 된 member를 add 시 score가 업데이트
127.0.0.1:6379> zadd fruit 15 apple
(integer) 0
 
# ZSCORE : member에 해당하는 score 값 리턴
127.0.0.1:6379> zscore fruit apple
"15"
 
# ZRANK : member에 해당하는 rank(순위) 값 리턴
127.0.0.1:6379> zrank fruit melon
(integer) 2
 
# ZRANGE : key에 해당하는 start - stop 내가 출력하고 싶은 요소를 추출
127.0.0.1:6379> zrange fruit 0 -1
1) "orange"
2) "watermelon"
3) "melon"
4) "banana"
5) "apple"
```


# Reference

---

- [https://inpa.tistory.com/entry/REDIS-📚-데이터-타입Collection-종류-정리](https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%83%80%EC%9E%85Collection-%EC%A2%85%EB%A5%98-%EC%A0%95%EB%A6%AC)