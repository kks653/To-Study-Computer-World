# :oil_drum: **Redis 개론** 

`Redis`는 C언어로 작성된 Key-Value 기반 In-Memory 데이터 구조 저장 공간으로 속도가 빠르고 데이터베이스, 캐시, 메시지 브로커, 스트리밍 엔진과 같은 역할로 쓰일 수 있다.

리눅스, *BSD, Mac OS X와 같은 거의 모든 POSIX 시스템에서 외부적인 의존없이 작동한다. Redis는 거의 리눅스와 OS X에서 개발 및 테스트 되었고 리눅스에 배포하는 것을 추천한다고 한다. 공식적인 윈도우 빌드 제공은 없다.

<br>

## 지원하는 자료 구조

- Strings
- Hashes
- Lists
- Sets
- Sorted sets
- Bitmaps
- Hyperloglogs
- Geospatial indexes
- Streams

<br>

Redis는 replication, Lua scriptiong, LRU eviction, transactions, 그리고 다양한 레벨의 on-disk 지속성을 내장하고 있고 지속적으로 안정적인 운영을 할 수 있는 고가용성(High Availability) Redis Sentinel과 자동으로 파티셔닝을 해주는 Redis Cluster를 내장하고 있다.

`Sorted set에서 가장 랭킹이 높은 멤버의 데이터를 가져오는 것`과 같은 작업 타입의 원자 조작(Atomic Operation)이 가능하다. 

그래서 동일한 key에 대해 작업을 수행하는 여러 개의 프로세스가 경쟁 상태(Race Condition)에 놓이지 않는다.

```
컴퓨터 과학에서 원자 조작(Atomic Operation, 原子操作)이라 함은 결합하여 외부에서는 하나의 조작으로 보이는 조작의 집합을 말한다.
원자 조작의 결과는 성공이나 실패 중의 하나이다.

- Wikipedia; Reference [2]
```


Redis는 또한 다음을 포함하고 있다:
- Pub/Sub(Public/Subscribe 메시징 패러다임) -> 작성해둔 [Message Queue의 문서](../../Network/MQ/MessageQueue/MessageQueue.md) 내용을 참조하면 좋다.
- Keys with a limited time to live(TTL, 컴퓨터나 네트워크에서 데이터의 유효 기간)
- 페일오버(failover, 장애 극복 기능; Redis Sentinel 기능 중 하나)

<br>

---
### **References**
[1] *Introduction to redis*. Redis. (n.d.). Retrieved July 6, 2022, from https://redis.io/docs/about/

[2] Wikimedia Foundation. (2022, February 1). *원자 조작*. Wikipedia. Retrieved July 6, 2022, from https://ko.wikipedia.org/wiki/%EC%9B%90%EC%9E%90_%EC%A1%B0%EC%9E%91 