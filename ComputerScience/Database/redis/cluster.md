# :footprints: **Redis Cluster**

Redis Cluster(이하 클러스터)는 자동으로 여러 Redis 노드로 수평적 스케일링(샤딩)이 가능하게 해준다.

클러스터는 파티션 간에 어느 정도의 가용성을 제공한다. 일부 노드가 실패하거나 통신할 수 없을 때 작업을 계속할 수 있게 해준다. 하지만 더 큰 오류가 발생하는 경우(i.g. 대부분의 마스터를 사용할 수 없는 경우) 작동을 중지한다.

파티션 간 가용성 부분은 Redis Sentinel의 자동 장애 극복(Automatic failover) 기능과 비슷해 보인다.

클러스터의 기능을 정리하자면,
- 데이터셋을 여러 노드로 `자동으로` 분할할 수 있는 기능
- 일부 노드에서 장애가 발생하거나 클러스터의 나머지 부분과 통신할 수 없을 때 작업을 계속할 수 있는 기능

### Redis Cluster data sharding

TBU

### Redis Cluster master-replica model

[Redis replication 참조!](replication.md)

TBU

<br>

---
### **References**
[1] *Scaling with Redis Cluster*. Redis. (n.d.). Retrieved July 12, 2022, from https://redis.io/docs/manual/scaling/