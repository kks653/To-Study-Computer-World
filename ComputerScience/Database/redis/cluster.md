# :footprints: **Redis Cluster**

Redis Cluster(이하 클러스터)는 자동으로 여러 Redis 노드로 수평적 스케일링(샤딩)이 가능하게 해준다.

클러스터는 파티션 간에 어느 정도의 가용성을 제공한다. 일부 노드가 실패하거나 통신할 수 없을 때 작업을 계속할 수 있게 해준다. 하지만 더 큰 오류가 발생하는 경우(i.g. 대부분의 마스터를 사용할 수 없는 경우) 작동을 중지한다.

파티션 간 가용성 부분은 Redis Sentinel의 자동 장애 극복(Automatic failover) 기능과 비슷해 보인다.

클러스터의 기능을 정리하자면,
- 데이터셋을 여러 노드로 `자동으로` 분할할 수 있는 기능
- 일부 노드에서 장애가 발생하거나 클러스터의 나머지 부분과 통신할 수 없을 때 작업을 계속할 수 있는 기능

### Redis Cluster data sharding

클러스터는 일관된 해시를 사용하지 않고 모든 키가 `해시 슬롯(hash slot)`이라고 부르는 것의 일부인 다른 형태의 샤딩을 사용한다.

클러스터에는 16384개의 해시 슬롯이 있고 주어진 키에 대한 해시 슬롯을 계산하기 위해 `key modulo 16384`(나머지 연산)의 CRC16(Cyclic Redundancy Check, 네트워크 데이터블록 또는 파일 등 데이터에 근거하여 짧고 간단하며 자리수가 고정 된 검증코드를 발생시키는 채널코딩기술이며 주로 데이터 전송 또는 저장 후에 발생할 수 있는 오류 등을 감지 또는 검증하는데 사용[2])을 가져가기만 하면 된다.

클러스터의 모든 노드는 해시 슬롯의 하위 집합을 담당하므로, 예를 들어 노드가 3개인 클러스터를 사용할 수 있다:

- 노드 A에는 0부터 5500까지의 해시 슬롯을 포함한다.
- 노드 B는 5501부터 11000까지의 해시 슬롯을 포함한다.
- 노드 C는 11001부터 16383까지의 해시 슬롯을 포함한다.

이로써 클러스터 노드의 추가와 삭제를 쉽게 한다. 예를 들어 노드 D를 추가하고 싶을 때 몇몇 해시 슬롯을 노드 A, B, C에서 D로 옮기면 된다. 비슷하게 노드 A를 없애고 싶다면 그냥 A에 있는 해시 슬롯을 B와 C로 옮기면 된다. 다 옮기고 노드 A가 빈 공간이 되면 클러스터에서 완전히 삭제된다.

<br>

### Redis Cluster master-replica model

[:bulb: Redis replication 참조!](replication.md)

TBU


<br>

### Redis Cluster consistency guarantees

<br>

---
### **References**
[1] *Scaling with Redis Cluster*. Redis. (n.d.). Retrieved July 12, 2022, from https://redis.io/docs/manual/scaling/

[2] *CRC-16*. Redis. (n.d.). Retrieved July 14, 2022, from http://wiki.hash.kr/index.php/CRC-16