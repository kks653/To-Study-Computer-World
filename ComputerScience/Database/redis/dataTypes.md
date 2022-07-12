# :ringed_planet: **Redis Data Types(자료형)** 

Redis는 단순히 key-value 저장소가 아니고 다양한 자료를 지원하는 자료 구조 서버이다. 기존에 있던 key-value 저장소들은 string key를 string value와 연관지어 사용해왔지만, redis에서는 value가 단지 string에 국한되지 않고 더 복잡한 자료 구조를 가질 수 있다.

- [Binary-safe strings](#keys)
- [Lists](#Lists)
- [Hashes](#hashes)
- [Sets](#sets)
- [Sorted sets](#sorted-sets)
- [Bit arrays(Bitmaps)](#bitmaps)
- [HyperLogLogs](#hyperloglogs)
- Streams


### Keys
Binary-safe라고 하면 어떠한 binary sequence라도 key가 될 수 있다는 것이다. 예를 들어, "foo"와 같은 string(문자열)부터 JPEG 파일까지 key가 될 수있다. 공백도 유효한 key가 될 수 있다(가능하지만 하지 않은 걸로...).

Key에 대한 또 다른 규칙:
- 너무 긴 길이의 key는 좋지 않다. 예를 들어, 1024바이트에 달하는 key는 메모리 측면에서도 좋지 않고 데이터셋의 key 검색을 할 때에도 key를 비교하는 큰 비용이 요구될 수 있다. 
- 너무 짧은 길이의 key는 대부분 좋지 않다. `u1000flw`보다 `user:1000:followers`와 같이 key 객체 자체와 value 객체가 사용하는 추가된 공간은 미미하고 좀 더 읽기 쉬운 쪽을 선택하는 것이 좋다. 물론 짧은 길이의 key가 메모리를 적게 차지하겠지만, 적절히 밸런스 있게 길이를 정하는 것이 당신의 몫이다.
- 스키마를 정해놓고 사용하는 것이 좋다. i.g. `object-type:id` -> `user:1000`
-  허용하는 key의 최대 크기는 512MB 이다.

<br>

### Strings
Redis String 타입은 Redis key로 쓸 수 있는 가장 심플한 타입이다.
Redis key가 string으로 되어 있기 때문에 value의 타입을 string으로 쓴다면 `string-string` 형태로 string끼리 맵핑된다.

```c
> set mykey somevalue
OK
> get mykey
"somevalue"
```

String 자료 타입은 유용하게 많은 용도로 사용되는데 HTML fragments나 페이지를 캐싱할 수도 있다.

<br>

### Key expiration
Key에 대한 유효/만료 시간(timeout)을 정할 수 있다.  `time to live` `TTL`이라고도 하는데 지정한 시간이 경과 되면 key는 자동으로 파괴된다.

Key expiration에 대한 몇 가지 중요한 사실:
- 초(seconds) 단위나 밀리초(millisecons) 정밀도로 지정할 수 있다.
- 하지만 만료 시간분해능([2]시간적으로 접근하여 입력하는 신호를 개별적으로 판단할 수 있는 최소의 입력신호에 대한 시간 간격)은 항상 1 millisecond 이다.
- 유효/만료에 대한 정보는 디스크에 복제되고 유지되며, Redis 서버가 중지된 상태로 있어도 사실상 시간이 경과하는 것과 같다(즉, Redis가 키가 만료되는 날짜를 저장하기 때문).

```c
> set key some-value
OK
> expire key 5
(integer) 1
> get key (immediately)
"some-value"
> get key (after some time)
(nil)
```

<br>

### Lists
- 삽입 순서에 따라 정렬된 string 요소 컬렉션이다. 기본적으로 `linked lists` 이다. Linkend list로 구현되어 있기 때문에 수 백만 개로 이루어진 리스트에 새로운 요소가 추가되어도 상수 시간에 처리된다.

<br>

### Hashes
`field-value` 구조로 예상하는 그 `hash`(이하 해시)와 같다.

```c
> hmset user:1000 username antirez birthyear 1977 verified 1
OK
> hget user:1000 username
"antirez"
> hget user:1000 birthyear
"1977"
> hgetall user:1000
1) "username"
2) "antirez"
3) "birthyear"
4) "1977"
5) "verified"
6) "1"
```

해시는 객체를 표현하는데 편리하지만 해시에 넣을 수 있는 field 수에는 실질적인 제한이 없어 다양한 방식으로 해시를 사용할 수 있다.

<br>

### Sets
TBU

<br>

### Sorted Sets
TBU

<br>

### Bitmaps
TBU

<br>

### HyperLogLogs
TBU

<br>

---
### **References**
[1] *Data types tutorial*. Redis. (n.d.). Retrieved July 12, 2022, from https://redis.io/docs/manual/data-types/data-types-tutorial/

[2] *시간분해능*. (n.d.). Retrieved July 12, 2022, from https://terms.naver.com/entry.naver?docId=664890&amp;cid=50327&amp;categoryId=50327 