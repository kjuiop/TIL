# 투표 

---

# 목차

---

- 투표기능
    - 예약 투표
    - 투표 제목, 내용, 이미지 지정
    - 기간 설정
    - 카테고리 설정 (홈, 커뮤니티, 종목, 테마 등)
    - 투표 항목 명칭 및 이미지 지정 가능
    - 단일 투표
    - 복수 투표
    - 선착순 투표

투표시스템 요구사항은 예약, 기간 설정, 단일 투표, 복수 투표, 선착순 투표 등의 기능이 포함됐습니다.


해당 요구사항을 받은 저는 ‘어떤 성격을 지닌 투표를 객체로 활용하면 좋을까?’ 하는 고민 시작

### 공통으로 적용할 수 있는 특성

---

- 예약 투표
- 단일 투표 / 복수 투표
- 기간 투표 / 무기한 투표
- 카테고리 설정
- 투표 / 투표 항목 제목, 내용, 이미지 지정

### 공통으로 적용하지 못하는 특성

---

- 일반 투표
- 선착순 투표

# 투표 시스템 아키텍처 요구사항

---

## 요구사항 1. 사용자가 투표를 하고, 서버는 투표 결과를 알려준다.

- 투표하겠습니다.
- 투표 완료했습니다. or 실패했습니다.

## 요구사항 2. 비즈니스가 언제든지 바뀔 수 있기 때문에 항상 다른 투표가 추가적으로 생기거나 삭제될 경우를 대비해야 한다.

- 일반투표
- 선착순투표
- 주주 투표
- 관심 종목 투표 등등..

## 요구사항 3. 여러명이 동시에 투표할 경우, Race Condition 이 발생하기 때문에 이를 대비해야 한다.

![Screen Shot 2022-11-06 at 12 05 23 PM](https://user-images.githubusercontent.com/41246605/200152524-cbd94e0b-1fd4-4768-b595-57811d9695b3.png)




- 사용자가 여러명이 동시에 투표하는 것이 왜 문제일까?
    - 100명이 동시에 투표를 하게된다면, DB에 100이라는 숫자가 더해지지 않고 100에 못 미치는 숫자의 값이 증가됩니다.
    - 한 번에 여러 쓰레드가 현재 카운트를 읽고, 현재 카운트에 1만 증가된 값으로 여러 쓰레드가 업데이트하기 때문이다. 위 그림과 같이 Thread4까지 1로 업데이트 하는 현상을 확인 할 수 있습니다.

<br />
<br />

# 투표 시스템 아키텍처

---


![Screen Shot 2022-11-06 at 12 08 23 PM](https://user-images.githubusercontent.com/41246605/200152531-a73460bc-da8a-44a8-ac71-08d66604e71a.png)

<br />

### 요구사항 1 에서 사용자는 투표를 하고, 서버는 투표 결과를 알려준다는 문구를 사용했습니다.

<br />

![Screen Shot 2022-11-06 at 12 09 10 PM](https://user-images.githubusercontent.com/41246605/200152532-8059d2b0-b6c5-4d5e-bea2-7d5ce6e5c9b0.png)



저희는 현재 Layerd Architecture 를 사용하고 있으며 Presentation Layer 와 Service Layer 사이에 각 Service 간 의존하는 것을 막기 위해 Facade Layer 를 추가적으로 사용하고 있습니다.

유저가 “투표합니다”라는 요청을 보낼 경우, Facade Layer에서도 하위 모듈(투표 구현체-일반투표, 선착순투표, 주주 투표, 관심 종목 투표 등)을 직접적으로 의존하는 것이 아닌 상위 모듈인 투표 타입 인터페이스를 의존해야 합니다.

이는 컴파일 레벨에서의 의존성을 고정시키지 않고, 런타임 레벨에서의 의존성을 변경시킬 수 있으며 DIP를 지킬 수 있는 설계가 됩니다.

### 요구사항 2 처럼 비즈니스는 항상 변동되기 때문에 투표가 늘어나거나 삭제되도  클라이언트 코드에 영향을 주지 않아야 합니다.

<br />
그렇기 때문에 투표타입이라는 인터페이스를 두고 일반투표(DefaultVote)라는 구현체를 베이스로 하나 두고, 추가적으로 확장되는 투표들을 Decorator 패턴을 사용해 OCP 를 위배하지 ㅇ낳고 다양한 투표를 추가/삭제할 수 있습니다.

Decorator 패턴을 선택한 이유는 선착순 투표, 주주투표 등 어떤 투표던 간에 기본적인 투표시스템이 갖춘 기능을 사용하기 위함입니다.

# 선착순 투표 아키텍처

---

![Screen Shot 2022-11-06 at 12 18 01 PM](https://user-images.githubusercontent.com/41246605/200152534-1b01a527-25d4-40f8-b93c-943baf3028d2.png)


### 요구사항 3 : 선착순 투표는 동시성을 제어해야 한다.

동시성을 제어하는 방법은 다양한 방법 (Redis Distributed Lock, DB Optimistic Lock, Pessimistic Lock, Named Lock 등 ) 이 존재합니다. 이번 포스팅은 동시성 제어에 초점이 맞춰진 포스팅이 아니기 때문에 각각의 특징을 설명하진 않겠습니다. 저는 이 중 저희 프로젝트 환경에서 가장 알맞은 Named Lock을 선택했습니다.

현재 프로젝트에서 Named Lock 을 처음 도입하는 것이기 때문에 팀원들도 다음에 편하게 사용할 수 있도록 NamedLockTemplate 을 구현했습니다. NamedLockStategy 구현체를 받아서 해당 구현체의 비즈니스 로직이 실행하기 전에 NamedLock 을 잡고 비즈니스 로직이 끝난 후에 NamedLock 을 해제하는 역할을 합니다.

추후에 투표라는 도메인이 아닌 다른 도메인에서도 NamedLock 은 Strategy 패턴을 사용해서 다양한 도메인에 적용할 수 있도록 합니다.



<br /><br />


# Step 1. 투표 타입 선택

---


![1](https://user-images.githubusercontent.com/41246605/200165722-d02fad23-98b9-4a6f-afec-c73be583de45.png)


### VoteFacade

```java
public class VoteFacade {
    ...

    public void vote(final VoteRequest voteRequest){
        final VoteVO findVote = findVoteService.findById(voteRequest.getVoteIdx());
        final VoteService voteService = voteServiceFactory.create(findVote);
      
        voteService.vote(findVote, voteRequest);
    }
}
```

- VoteFacade 에서는 유저가 요청한 투표 를 DB에서 가져옵니다.
- VoteFacade 는 유저가 일반투표, 선착순 투표, 주주 투표 중에서 어떤 투표를 하든 알 필요가 없습니다. 그저 유저가 투표를 한다는 메세지를 전달하고 투표의 결과를 전달하면 됩니다.
- 그렇기 때문에 특정 하위모듈을 의존하지 않고 VoteService 라는 상위 모듈인 인터페이스를 의존하게 됩니다.

### VoteService

```java
@FunctionalInterface
public interface VoteService {
    void vote(VoteVO vote, VoteRequest voteRequest);
}
```

- VoteService 는 인터페이스로 구성되어 있으며 VoteService 의 구현체들 (일반투표, 선착순 투표 등)은 투표하다라는 행위를 해야하기 때문에 vote 라는 메소드가 존재합니다.

### VoteServiceFactory

```java
public class VoteServiceFactory {
    private final DefaultVoteService defaultVoteService;
    private final NamedLockVoteFirstComeStrategy namedLockVoteFirstComeStrategy;

    public VoteService create(final VoteVO vote){
        return vote.isFirstCome() // 선착순 투표 여부
                ? namedLockVoteFirstComeStrategy
                : defaultVoteService;
    }
}
```

- VoteServiceFactory 에서는 투표를 받아서 해당 투표의 구현체를 리턴하는 역할을 합니다.
- 현재는 일반투표 (DefaultVote) 와 선착순 투표 (FirstComeVote) 만 존재하기 때문에 두 구현체를 리턴해주게 됩니다. 만약 투표 타입이 추가된다면 클라이언트인 Facade의 코드는 변경하지 않아도 되고 VoteServiceFactory에 해당 구현체를 추가하고 추가적인 투표타입의 구현체를 구현해주면 됩니다.
- 요청에 따라 구현체가 달라지기 때문에 VoteServiceFactory 는 컴파일 의존성이 아닌 런타임 레벨에서 의존성에 의존하는 DIP를 충족할 수 있습니다.



# Step 2. 투표 타입 구현체

---

![2](https://user-images.githubusercontent.com/41246605/200165758-35f77476-a878-4f6b-962c-07bc78099707.png)


- VoteService 를 구현한 일반투표 (DeafultVote) 를 베이스로 구현체로 생성했습니다. 또한 일반 투표를 공통적으로 사용하고 더 확정적으로 추가될 투표를 위한 투표 데코레이터 (Decorator) 를 생성하고 이를 상속받은 선착순 투표 (FirstComeVote) 를 소개합니다.

### DefaultVoteService - 일반투표

```java
public class DefaultVoteService implements VoteService{
	...

	@Override
	public void vote(final VoteVO vote, final VoteRequest voteRequest) {
	  vote.validReserved();
	  vote.validClosed();
	  vote.validPeriod();
	  vote.validMultipleSelection();

	  incrementVoter()
	  saveVoteResult();
	}
	...
}
```

- VoteService 를 구현한 일반투표(DefaultVote)이며, 모든 투표의 공통적인 기능을 담당합니다.
- 예약시간, 투표 마감, 투표 기간, 복수 투표 등을 Validation 을 담당합니다. 또한, 투표 카운트를 증가시키며 투표 결과를 저장합니다.

### VoteServiceDecorator - 투표 데코레이터

```java
public abstract class VoteServiceDecorator implements VoteService {
		private DefaultVoteService defaultVoteService;

    public VoteServiceDecorator(DefaultVoteService defaultVoteService) {
        this.defaultVoteService = defaultVoteService;
    }

    @Override
    public void vote(final VoteVO vote, final VoteRequest voteRequest) {
        before(vote);
        defaultVoteService.vote(vote, voteRequest);
        after(vote);
    }

    public abstract void before(VoteVO vote);

    public abstract void after(VoteVO vote);
}
```

- 확장적인 투표를 위한 VoteServiceDecorator 이며, 일반투표 (DefaultVote)를 기반으로 before 추상 메서드와 after 추상 메서드를 가지고 있습니다.
- VoteService 를 구현하기 때문에 기존 코드의 변경 없이 VoteServiceDecorator 를 상속받아서 추가적인 투표 타입을 구현할 수 있습니다.
- before 와 after 추상 메서드를 상속받은 투표 타입에서 구현해줌으로써 각 투표 책임에 맞는 로직을 구현할 수 있습니다.

### FirstComeVoteService - 선착순 투표

```java
public class FirstComeVoteService extends VoteServiceDecorator {
    ...

    public FirstComeVoteService(DefaultVoteService defaultVoteService, ...) {
        super(defaultVoteService);
        ...
    }

    @Override
    public void before(final VoteVO vote) {
        final Long currentVotedCount = voteMemberCountRepository.findCountByIdx(vote.getIdx());
        vote.validFirstCome(currentVotedCount);
    }

    @Override
    public void after(final VoteVO vote) {
        final Long countOfAfterVoted = voteMemberCountRepository.findCountByIdx(vote.getIdx());

        if(vote.isMaxOfVotersCount(countOfAfterVoted) && !vote.isClosed()){
            vote.closed(); // 투표 상태 "CLOSED"로 변경
            updateVoteStatus(vote);
            saveStatusHistory(vote);
        }
    }
    ...
}
```

- VoteServiceDecorator 를 상속받은 선착순 투표 (FirstComeVote) 입니다.
- before 메서드에서는 현재 투표 수를 검증하기 위한 validation을 구현했습니다.
- after 메서드에서는 투표 후 카운트를 체크하여 투표 상태를 CLOSED 로 업데이트하며 이를 변경, 저장하는 로직을 구현했습니다.

> before 와 after 추상 메서드를 구현함으로써, 확장돠는 투표의 성격을 커스텀마이징 할 수 있습니다. 선착순  투표는 before 에 현재 투표자를 검증하고, after에 투표 후 카운트를 확인해서 투표 상태를 바꾸는 것을 확인할 수 있습니다. Decorator 를 상속 받아서 주주만 투표할 수 있는 주주 투투와 관심종목을 설정한 사람만 투표할 수 있는 관심 종목 투표를 추가적으로 알아보겠습니다.
>

### StockHolderVoteService - 주주 투표

```java
public class StockHolderVoteService extends VoteServiceDecorator {
    ...

    public StockHolderVoteService(DefaultVoteService defaultVoteService, ...) {
        super(defaultVoteService);
        ...
    }

    @Override
    public void before(final VoteVO vote) {
        if(!hasStock()){
            ...  
        }
    }

    @Override
    public void after(final VoteVO vote) {
        save...();
        update...();
    }
    ...
}
```

- VoteServiceDecorator 를 상속받은 주주 투표 (StockHolderVote) 입니다.
- before 메서드에선 해당 주식을 가지고 있는 주주인지 판별하는 validation 을 구현했습니다.
- after 메서드엔 특정 종목에 대한 주주 활동 데이털르 넣는 등 추가적인 후처리방식을 구현했습니다.

### FavoriteStockVoteService - 관심 종목 투표

```java
public class FavoriteStockVoteService extends VoteServiceDecorator {
    ...

    public FavoriteStockVoteService(DefaultVoteService defaultVoteService, ...) {
        super(defaultVoteService);
        ...
    }

    @Override
    public void before(final VoteVO vote) {
        validFavoriteStock();
    }

    @Override
    public void after(final VoteVO vote) {
        save...();
        update...();
    }
    ...
}
```

- VoteServiceDecorator 를 상속받은 관심 종목 투표 (FavoriteStockVote) 입니다.
- before 메서드에선 해당 주식을 관심 종목으로 설정했는지에 대한 validation 을 구현했습니다.
- after 메서드엔 특정 종목을 관심 종목으로 설정한 유저 활동 데이터를 넣는 등 추가적인 후처리방식을 구현했습니다.

> Decorator 를 사용함으로써 추가적인 투표나 기존 투표를 수정함에 있어서 클라이언트 투표를 수정하지 않고 전처리, 후처리를 구현할 수 있습니다. 주의할 점은, 상속을 사용하면 하위 객체들은 상위 객체에 영향을 받기 때문에 추후에 상위 객체가 변동하지 않는 비즈니스에 적합하다고 생각합니다. 이 때문에 Decorator 패턴 또한 신중히 사용해야 합니다.


<br /><br />


# Step 3. 선착순 투표 동시성 제어

---

### NamedLock 이란?

![3](https://user-images.githubusercontent.com/41246605/200165805-e2c0203e-542c-45cb-8310-9d9baeb8c0d6.png)


- 다른 데이터베이스 벤더사에도 존재하는지 모르지만, 저희팀이 사용하고 있는 Mysql 기준 Named Lock 을 설명하겠습니다.
- Named Lock 은 레코드와 테이블 단위 또는 데이터베이스 객체에 락을 거는 것이 아니라, `GET_LOCK()` 함수를 이용해 임의의 문자열에 대한 잠금을 설정합니다.
- 위 그림과 같이 여러 세션이 한 번에 `ZUM-TECH` 라는 문자열에 `GET_LOCK()` 으로 잠금을 시도하는 경우, 락이 해제 (Release) 된 경우에는 가장 먼저온 세션이 획득하게 됩니다. 추후에 락이 해제되고, 락을 기다린 세션의 순서는 잠금 획득을 시도한 순서와 다를 수 있습니다.
- 추후에 선착순 이벤트와 선착순 예매 등 다양한 usecase 에도 활용될 수 있을 것 같습니다.


![4](https://user-images.githubusercontent.com/41246605/200165814-149cfdfd-5f05-411b-8968-d70dfe298466.png)


동시성을 제어하기 위한 NamedLockTemplate 과 NamedLockStrategy, 선착순 투표를 위해 이를 구현한 NamedLockFirstComeVoteStrategy 를 소개합니다.


### NamedLockStrategy - 네임드 전략

```java
@FunctionalInterface
public interface NamedLockStrategy {
    void business(NamedLockDTO namedLockDTO);
}
```

- Race Condition 이 발생하기 때문에 이를 제어하기 위한 NamedLockStrategy 인터페이스입니다.
- Named Lock 을 사용하는 곳에서 NamedLockStrategy 를 구현해서 비즈니스 로직을 추가합니다. 비즈니스 로직은 NamedLockTemplate 과 함께 사용됩니다.

### NamedLockFirstComeVoteStrategy - 선착순 투표 네임드락 전략

```java
public class NamedLockVoteFirstComeStrategy implements VoteService, NamedLockStrategy {
    private final FirstComeVoteService firstComeVoteService;
    private final NamedLockTemplate namedLockTemplate;
    ...
    
    @Override
    public void vote(final VoteVO vote, final VoteRequest voteRequest) {
        final NamedLockVoteFirstComeDTO namedLockVoteFirstComeDTO = new NamedLockVoteFirstComeDTO(vote, voteRequest);
        final String lockName = LOCK_NAME_PRE + "_" + vote.getIdx();

        namedLockTemplate.businessWithLock(this, namedLockVoteFirstComeDTO, lockName);
    }

    @Override
    public void business(final NamedLockDTO namedLockDTO) {
        final NamedLockVoteFirstComeDTO namedLockVoteFirstComeDTO = (NamedLockVoteFirstComeDTO) namedLockDTO;

        firstComeVoteService.vote(namedLockVoteFirstComeDTO.getVote(),namedLockVoteFirstComeDTO.getVoteRequest());
    }
    ...
}
```

- NamedLockFirstComeVoteStrategy 는 VoteService 과 NamedLockStrategy 인터페이스를 구현합니다.
- 동시성 제어를 위해 vote 메서드에 네임드락에서 사용할 락 이름을 지정하고, NamedLockTemplate.businessWithLock 이라는 메서드를 호출합니다. (NamedLockTemplate 은 바로 아래에서 설명합니다.)
- NamedLockStrategy 를 오버라이드한 business 메서드에선 선착순 투표를 호출합니다.

### NamedLockTemplate - 네임드락 템플릿

```java
public class NamedLockTemplate {

    private final NamedLockRepository namedLockRepository;
    private final DataSource serviceDatasource;

    public void businessWithLock(final NamedLockStrategy namedLockStrategy, final NamedLockDTO namedLockDTO, final String lockName) {
        Connection connection = null;
        try {
            connection = DataSourceUtils.getConnection(serviceDatasource);
            connection.setAutoCommit(false);

            namedLockRepository.getLock(connection, lockName, 3);
            namedLockStrategy.business(namedLockDTO);

            connection.commit();
        } catch (Exception e) {
            if(connection != null) {
                try {
                    connection.rollback();
                    ...
                } catch (SQLException sqlException) {
                    ...			
                }
            }
        }
        finally {
            namedLockRepository.releaseLock(lockName);
        }
    }
```


- NamedLockTemplate Flow
  - Named Lock을 선점
  - NamedLockStrategy 구현체를 받아와서 해당 business 메서드 실행
  - 커밋 or 롤백
  - Named Lock 해제
  

- NamedLockTemplate.businessWithLock 메서드의 매개변수로 NamedLockStrategy 을 받는 것을 확인할 수 있습니다. 그렇기 때문에 선착순 투표가 아닌 다른 도메인에서도 NamedLockStrategy를 구현해서 유연하게 NamedLockTemplate 을 사용할 수 있습니다.


- NamedLockTemplate 에서는 한 커넥션에서 getLock() 과 releaseLock() 을 하는 것 핵심입니다. 그렇기 때문에 DataSourceUtils 에서 커넥션을 가져오는 것을 확인할 수 있습니다. DataSourceUtils 는 TransactionSynchronizationManager 에서 ThreadLocal 로 커넥션을 가지고 있기 때문에 다른 유저들과 중복되지 않는 고유한 커넥션을 가져올 수 있습니다.


- 위 코드를 보면 커넥션을 가지고 와서 수동 커밋모드로 변경하고 직접 수동 커밋과 롤백하는 것을 확인할 수 있습니다. 그렇다면 왜 수동 커밋과 수동 롤백을 하는 것일까요?

<br />

![5](https://user-images.githubusercontent.com/41246605/200165844-eb94c0d8-796c-41e8-b01e-505c431015af.png)


- 정답은 동시성 문제에 있습니다. 특정 메서드에서 @Transactional 을 사용하면 해당 메서드의 블록이 끝날 때 TransactionManager 에서 커밋 or 롤백을 진행하게 됩니다. 하지만 NamedLock 을 사용할 경우 여러 커넥션이 NamedLock 을 선점하기 위해 대기하다가 락을 점유했던 세션이 락을 해제하게 되면 바로 다음 세션이 락을 점유하는 방식입니다.
- 문제는 바로 여기에 있습니다. 후행 세션이 락을 점유할 때 TransactionManager 에서 커밋 or 롤백하는 시점보다 더 빠르게 락을 점유해서 이전 데이터를 읽었기 때문입니다. 위 그림과 같이 3번으로 가서 커밋 or 롤백하는 시간보다 2번에서 Lock 을 release 하고 acquire 하는 시간이 더 빠르기 때문에 동시성 문제가 생깁니다.
- 이 문제 때문에 DataSourceUtils 에서 직접 커넥션을 가져와서 수동 커밋, 수동 롤백하는 방법으로 동시성 문제를 해결했습니다.

<br /><br />

### NamedLockRepository

```java
public class NamedLockRepository {

    private ThreadLocal<DSLContext> namedLockContext = new ThreadLocal<>();

    public void getLock(Connection connection, String lockName, int timeOutSeconds) {
        ...
        final DSLContext newNamedLockDSLContext = DSL.using(connection, dialect, settings);

        newNamedLockDSLContext.fetch("SELECT GET_LOCK(?,?)", lockName, timeOutSeconds);
        namedLockContext.set(newNamedLockDSLContext);
    }

    public void releaseLock(String lockName){
        try {
            namedLockContext.get().fetch("SELECT RELEASE_LOCK(?)",lockName);
        }finally {
            namedLockContext.remove(); 
        }
    }
}
```

- NamedLockRepository 에서는 jOOQ를 사용했기 때문에 같은 커넥션으로 동일한 DSLContext 에서 Lock을 acquire 하고 release 하는 것이 핵심입니다.
- 한 Thread 에서 동일한 DSLContext 를 사용해야하기 때문에 ThreadLocal 을 이용해서 동일한 namedLockContext 를 사용합니다.


<br /><br />

# 테스트 결과

---

![6](https://user-images.githubusercontent.com/41246605/200165880-395057d3-bc06-4fc3-8fb0-991ea089abfa.png)





# Reference

---

- [https://zuminternet.github.io/vote-architecture/](https://zuminternet.github.io/vote-architecture/)


