# :bookmark_tabs: **Redis 트랜잭션(Transactions)**

Redis Transactions(이하 Transactions)는 명령어 그룹을 한 번에 처리할 수 있도록 해준다.

관련 명령어 목록:
- MULTI
- EXEC
- DISCARD
- WATCH

### Transactions는 중요한 두 가지를 보장해준다.

- 트랜잭션의 모든 명령은 순차적으로 직렬화되고 실행된다. 다른 클라이언트의 요청이 오면 `절대` 실행 중간에 그 요청이 실행되지 않는다. 이것은 명령어가 단일 분리된 작업으로 실행되는 것을 보장한다.
- `EXEC` 명령어는 트랜잭션의 모든 명령을 실행시킨다. 클라이언트가 `EXEC` 명령을 호출하기 전에 트랜잭션의 컨텍스트에서 서버와의 연결이 끊긴 다면 모든 작업은 실행되지 않는다.

### 사용 방법

```c
> MULTI
OK
> INCR foo
QUEUED
> INCR bar
QUEUED
> EXEC
1) (integer) 1
2) (integer) 1
```

`MULTI`라는 명령어를 치면 `OK`라는 싸인과 함께 여러 명령어를 받을 준비가 된다. 바로바로 실행되던 것과 달리 Redis는 명령어를 큐(Queue)에 넣는다. `EXEC` 명령어가 들어올 때 실행된다.

`DISCARD` 명령어는 큐에 있던 명령어를 전부 없애 버리고 그 트랜잭션은 종료된다.

<br>

## **Optimistic locking using CAS(check-and-set)**

`WATCH` 명령어는 CAS(Check-And-Set) 작업을 Redis transactions에서 할 수 있도록 해준다.

`WATCH`ed key(`WATCH`로 지정한 key)는 변경 사항이 있는지 모니터링된다. `EXEC` 명령 전에 watched key가 하나라도 수정이 되면 전체 트랜잭션이 중단되고 `EXEC`는 트랜잭션이 실패했음을 알리는 Null 응답을 반환한다.

이와 같은 시도를 할 수 있다.
```c
val = GET mykey
val = val + 1
SET mykey $val
```

이 작업은 주어진 시간에 하나의 클라이언트에서만 수행을 한다면 안정적으로 돌아간다. 여러 개의 클라이언트가 거의 동시에 key를 증가시키려고 하면 서로 `경쟁 상태(race condition)`에 놓이게 된다.

예를 들어, 클라이언트 A와 B가 이전 값(10이라고 치자)을 읽었다. 두 클라이언트 모두 그 값을 증가시키고 마지막에 `SET`이라는 명령어를 통해 key의 값으로 저장한다. 이렇게 하면 결과 값은 `12`가 아닌 `11`이 된다.

`WATCH`를 이용하면 이 문제를 잘 다룰 수 있게 된다.
```c
WATCH mykey
val = GET mykey
val = val + 1
MULTI
SET mykey $val
EXEC
```

위와 같이 사용하면, 경쟁 상태(race condition)가 발생하고  `WATCH`와 `EXEC`의 호출 사이에 다른 클라이언트가 val의 결과를 변경한다면 그 트랜잭션은 실패하게 된다.

간단하게 이 작업을 경쟁 상태에 놓이지 않기를 바라며 반복하면 된다. 이런 식의 락킹(locking)을 일컬어 `optimistic locking(낙관적 락킹)`이라 한다.


<br>

---
### **References**
[1] *Transactions*. Redis. (n.d.). Retrieved July 7, 2022, from https://redis.io/docs/manual/transactions/ 