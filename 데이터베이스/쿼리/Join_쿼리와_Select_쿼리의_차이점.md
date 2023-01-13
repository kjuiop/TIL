### Join 쿼리와 Select 쿼리 중 어떤 것이 더 효율적일까?

- Join 쿼리는 여러 테이블을 합치는 작업을 수행할 때 사용하며, 일반 쿼리는 하나의 테이블에서 데이터를 가져오는 작업을 수행할 때 사용한다.
- Join 쿼리는 여러 테이블에서 필요한 데이터를 한 번에 가져오는 경우 효율적일 수 있지만, 테이블이 많고 복잡할수록 성능상의 문제가 발생할 수 있다.
- 이러한 경우에는 성능을 향상시키기 위해 필요한 테이블만 join 하는 것을 고려하는 것이 좋다.

- 반면에 일반 쿼리는 하나의 테이블만 조작하는 경우 성능상 이슈가 없을 수 있다.

<br />

### Join 쿼리를 사용할 때 성능상의 문제

- Cartesian product : 두 테이블을 전부 결합하면 행의 수가 곱해지므로, 테이블의 크기가 커질수록 결과로 반환되는 행의 수도 기하급수적으로 증가하여 성능이 저하될 수 있다.
- Table Scan : 테이블에 인덱스가 없거나 잘못된 인덱스가 사용되면 테이블 전체를 스캔해야 하므로, 성능이 저하될 수 있다.
- Data Type Mismatch : Join 조건에서 데이터 타입이 맞지 않으면 성능이 저하될 수 있다.
- Over-Join : 필요한 테이블만 Join 하지 않고 필요없는 테이블도 Join 하면 불필요한 행이 많이 생성되어 성능이 저하될 수 있다.
- Join 에 사용되는 테이블의 크기가 매우 크면 메모리를 많이 사용하여 메모리가 부족할 수 있고 또는 속도가 느려질 수 있다.

<br />

### Join 쿼리를 사용할 때의 성능상 문제에 대해 해결하는 방법

- 적절한 인덱스 사용 : Join 조건에 해당하는 컬럼에 인덱스를 생성해서 사용하면 성능을 향상시킬 수 있다.
- 적절한 Join 조건 사용 : 필요한 테이블만 join 하는 것을 고려하고, join 조건에서 가능한 경우 unique 키를 사용하면 성능을 향상시킬 수 있다.
- Join 테이블 순서 : 크기가 작은 테이블을 앞에 join 하면 성능을 향상시킬 수 있다.
- Limit : 필요한 결과만 가져오도록 limit을 사용하면 성능을 향상시킬 수 있다.
- Sub-query : join 이 너무 복잡하거나 큰 테이블에서 필요한 데이터만 가져오려면 sub-query를 사용하면 성능을 향상시킬 수 있다.
- 추가적으로 복잡한 join 을 사용할 경우, Join 테이블을 미리 생성해두거나 Materialized View를 사용하면 성능을 향상시킬 수 있다.
- 마지막으로, Join 쿼리를 실행할 때 서버의 리소스가 부족하지 않는지 확인하는 것도 중요하다. 메모리, CPU, 디스크 I/O 등 서버 자원을 확인하고 최적화할 필요가 있을 수 있다.

---

# Join 쿼리 동작방식 예시 

<br />

```sql
select c.challenge_id, c.user_id, c.habit_id, h.subtitle, count(auth_id) + count(wildcard_id)
from challenge c
left outer join habit h on c.habit_id = h.habit_id
left outer join auth a on c.challenge_id = a.challenge_id
left outer join wildcard w on c.challenge_id = w.challenge_id
group by challenge_id
```

1. From 절 : ‘challenge’ 테이블을 선택하고, ‘habit’ 테이블과 left outer join 연산을 수행하여 조인 합니다.
2. on 절 : challenge 테이블과 habit 테이블을 habit_id 컬럼을 기준으로 조인합니다.
    1. challenge 테이블과 auth 테이블을 challenge_id 컬럼을 기준으로 조인합니다.
    2. challenge 테이블과 wildcard 테이블을 challenge_id 컬럼을 기준으로 조인합니다.
3. select 절 : challenge 테이블의 challenge_id, user_id, habit_id 컬럼, habit 테이블의 subtitle 컬럼, auth 테이블의 auth_id 컬럼의 개수 , wildcard 테이블의 wildcard_id 컬럼의 개수를 선택합니다.
4. group by 절 : challenge_id 컬럼을 기준으로 그룹을 만듭니다.
5. where, having, order by, Limit 절: 에 대한 연산을 수행합니다.


