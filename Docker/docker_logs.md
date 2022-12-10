# Docker logs

---


- --details : 로그에 추가적인 세부 정보를 표시합니다.
- --follow, -f : 로그를 출력한 후 커멘드를 종료하지 않고,interrupt 가 일어날 때까지 그대로 유지합니다.
- --since : 특정 타임스탬프 이후, 또는 상대적인 시간 이후 (42m, 5h) 의 로그만 출력합니다.
- --tail : 최근 순으로 n개의 로그만 가져오기 위해 사용합니다.
- --timestamps, -t : 로그에 타임스탬프를 함께 표시합니다.
- --until : 특정 타임스탬프 이전, 또는 상대적인 시간 이전의 로그만 출력합니다.

```go
docker logs -t {container_id} --tail 500

docker logs -t {container_id} --since 2022-12-09T04:37:05 --tail 50

docker logs {container_id} --tail 1000 > /tmp/{filename}
```


