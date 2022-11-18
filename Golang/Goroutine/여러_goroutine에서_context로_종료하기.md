## Context 의 Cancelation

---

Go 에서는 동시에 처리해야 하는 작업을 고루틴으로 실행한다. 이때 가장 주의해야 할 점은 내가 실행한 고루틴이 일정 시간 안에 반드시 종료될 것이란 것을 보장해야 한다는 것이다.

컨텍스트의 cancelation 기능을 사용하면 고루틴의 생명주기를 쉽게 제어할 수 있다.

```java
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
```

- 첫 번째 리턴값 `ctx` 는 새로 생성된 컨텍스트이다.
- 두 번째 리턴값 `cancel` 은 컨텍스트에 종료 신호를 보낼 수 있는 함수이다.

```java
type Context interface {
	Done() <-chan struct{}
	Err() error
}
```

- 컨텍스트의 `Done()` 메서드는 종료 신호를 전달받을 수 있는 채널을 반환한다.
- `cancel` 함수를 실행하여 컨텍스트에 종료 신호를 보내면 그 상황을 컨텍스트의 `Done()` 메서드를 통해 알 수 있는 것이다.
- `Err()` 메서드는 컨텍스트가 강제 종료되었을 때의 상황을 리턴한다.

그러면 우리는 우리가 생성한 Go Routine 에 대해 Context 의 `CancelFunc` 를 적용하여 동시 종료를 해보도록 하겠다.

# 여러 Go Routine 에서 Context 로 종료하기

---

여러 Go Routine 을 종료하기 위해서는 어떤 점들을 고려해야 할까?

1. 종료 시그널을 받을 수 있는 채널이 필요
    1. `chan os.signal` 로 채널 생성 및 등록
2. Sub 고루틴이 모두 종료된 이후 메인 고루틴의 종료
    1. `sync` 패키지의 `waitgroup` 활용

### Main.go

```java
func main() {
	
	wg := sync.WaitGroup{}
  
	sigs := make(chan os.Signal, 1)
	defer close(sigs)

	signal.Notify(sigs, syscall.SIGINT, syscall.SIGTERM, syscall.SIGQUIT, syscall.SIGKILL)

	// scheduler
	s, err := scheduler.NewScheduler()
	if err != nil {
		log.Println("[main] failed NewScheduler :", err)
		os.Exit(1)
	}
	defer s.Close()

	wg.Add(1)
	go s.CloseWithContext(sigs, &wg)

	wg.Add(1)
	go s.ExampleGoroutine2(&wg)

	wg.Add(1)
	go s.ExampleGoroutine3(&wg)

	wg.Wait()
}
```

- 먼저 우리는 wg 을 생성해준다.
    - 이는 우리가 서브 고루틴을 생성할 때마다 wg 객체를 포인터로 활용할 수 있다.
- sigs 라는 `chan os.Signal` 이라는 채널을 생성해 준 후 Notify 를 해준다.
    - 이때 주의할 점은 채널은 데이터를 한 번 송신하고, 이를 1번 수신받으면 소멸한다는 점이다.
    - 우리는 `ctrl + c` 를 한 번만 호출할 것이기 때문에 우리가 받는 종료시그널은 한번만 사용 가능하다.
- 메인 함수에서 동작할 scheduler 객체를 생성해준다.
    - 만일 메인 고루틴이 종료시에 scheduler 객체는 close 를 시켜준다.
- 우리는 3개의 고루틴을 생성하였다.
- wg.Add(1) 은 우리가 관리해야 할 wg 를 가시적으로 볼 수 있게 실행하고자 하는 고루틴 앞에 써준다.
- wg.wait() 를 선언함으로써 wg.Done() 이 3번 모두 호출된 이후에 종료될 수 있도록 한다.

### Schedule.go

```java
type Scheduler struct {
	gCtx    context.Context
	gCancel context.CancelFunc
}

func NewScheduler() (*Scheduler, error) {
	s := new(Scheduler)

	ctx, cancel := context.WithCancel(context.Background())
	s.gCtx = ctx
	s.gCancel = cancel

  return s, nil
}
```

- scheudler.go 에서는 sheduler 객체를 선언하고, context 를 생성하여 객체에 주입시켜준다.

```java
func (s *Scheduler) CloseWithContext() {
	for {
		select {
		case <-sigs:
			s.logger.TextLogger.Info("Receive exit signal")
			s.gCancel()
		case <-s.GCtx.Done():
			s.logger.TextLogger.Info("CloseWithContext Close Goroutine")
			s.logger.TextLogger.Info("CloseWithContext Close Goroutine")
			wg.Done()
			return
		default:
			s.logger.TextLogger.Info("CloseWithContext Check Goroutine running")
			time.Sleep(time.Second * 1)
		}
	}
}
```

- `CloseWithContext` 은 메인 고루틴에서 종료시그널을 받았을 때 Context 의 cancel() 를 실행시키는 고루틴이다.
    - `gCancel()` 은 scheduler 객체를 생성할 때 생성한 `context cancel func` 이다.
- 각 고루틴은 `case ← s.gCtx.Done()` 을 받아 context의 종료 시그널을 공유한다.
- 이로 인해 선언되었던 고루틴은 모두 종료할 수 있다.


## TEST

---

![1](https://user-images.githubusercontent.com/41246605/202712705-9dfe3305-3bdb-4c04-a233-6e6f6b5cf4f6.png)

- 3개의 고루틴을 작동시켰을 때의 로깅

![2](https://user-images.githubusercontent.com/41246605/202712717-ff383618-9dec-47f7-abd6-ca23007bafd9.png)

- 종료 시그널을 보냈을 때, 3개의 고루틴이 동시 종료되는 로깅


# Reference

---

- [https://jaehue.github.io/post/how-to-use-golang-context/](https://jaehue.github.io/post/how-to-use-golang-context/)
- [https://gist.github.com/fracasula/b579d52daf15426e58aa133d0340ccb0](https://gist.github.com/fracasula/b579d52daf15426e58aa133d0340ccb0)


