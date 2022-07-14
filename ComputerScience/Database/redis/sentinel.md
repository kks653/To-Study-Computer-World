# :eyes: **Redis 센티넬(Sentinel)**

Redis Sentinel(이하 Sentinel)은 Redis Cluster를 사용하지 않을 때 고가용성(안정성 UP)을 제공한다.

큰 그림으로 보는 Sentinel의 전체 기능 목록:
- Monitoring 모니터링
    - 끊임없이 마스터와 레플리카가 잘 작동하고 있는지 확인한다.
- Notification 알림
    - Redis에 문제가 생기면 API를 통해 시스템 관리자나 프로그램에 알림을 준다.
- Automatic failover 자동 장애 극복
    - 마스터 개체가 제대로 작동하지 않으면 Sentinel은 레플리카를 마스터로 승격시키고 다른 레플리카는 새 마스터를 사용하도록 재구성된다. Redis 서버를 사용하고 있는 애플리케이션은 연결할 새 주소 정보를 받는다.
- Configuration provider 환경 설정
    - 클라이언트는 주어진 서비스를 담당하는 현재 Redis 마스터의 주소를 묻기 위해 Sentinel에 연결한다. 장애 조치가 발생하면 Sentinel은 새 주소를 보고한다.

<br>

---
### **References**
[1] *High availability with Redis Sentinel*. Redis. (n.d.). Retrieved July 11, 2022, from https://redis.io/docs/manual/sentinel/