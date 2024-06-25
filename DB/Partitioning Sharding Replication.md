## Partitioning
### Vertical Partitioning
- column을 기준으로 table을 나누는 방식 
### Horizontal Partitioning
- row를 기준으로 table을 나누는 방식
- 하나의 DB 서버에 저장됨 (S)
- Range Partition
- List Partition
- Hash Partition
	- partition key를 기준으로 조회하게 됨 (where문의 인자)
		- partition key가 아닌 것을 기준으로 조회할 때 모든 테이블을 다 봐야함
		- 가장 많이 쓰이는 필드를 partition key로 정의하는 것이 중요
	- 데이터가 균등하게 분배될 수 있도록 hash function을 잘 정의하는 것이 중요
		- 기존의 partition에서 추가하는 것이 매우 까다로움 (첫 정의가 매우 중요)
		- 

## Replication
- 실제 database storage를 복제하는 것
	- 하나의 DB서버가 있었다면 새로운 서버를 copy해서 만듦
	- 끊임없이 copy를 해서 두 개의 데이터베이스를 동기화 시킴
- Primary Server - Secondary Server (수직적 구조)
- 장점
	- 서버에서 주로 read가 많이 일어나기 때문에 secondary 서버로 read를 분산하여 서버에 부담을 줄일 수 있음
	- 비동기 방식으로 지연 시간이 거의 없음
- 단점
	- SPOF (Single Point Of Failure)에 취약함 - Primary 서버가 다운되면 기능을 하지 못함
	- 서버 간 데이터 동기화가 보장되지 않음 - 일관성 있는 데이터를 얻지 못할 수 있음

## Clustering
- 여러 개의 DB를 수평적 구조로 구축
- 동기 방식으로 서버 간 데이터 동기화 -> SPOF 방지 && Fail Over 시스템 구축
- 

## Sharding
- horizontal partitioning처럼 동작
- 각 partition이 **독립된 DB**에 저장됨
- 테이블을 나눠서 검색
- Shard Key
	- Hash Sharding
		- 구현이 간단함 - 샤드의 수 만큼 hashing (key를 기준)
		- 데이터 정합성이 깨짐 (확장성이 떨어짐) - 샤드가 늘어나면 hash 함수가 달라져야 함
		- 공간복잡도를 고려하지 않음 - 
	- Dynamic Sharding
		- locator service (테이블)
			- id range - shard key
		- 샤드들이 locator service에 종속적 -> locator 서비스의 문제가 샤드 전체에 전이될 수 있음
	- Entity Group (RDB에 적합)
		- 관계성이 있는 테이블을 같은 샤드 내에 공유
		- 단일 샤드 내에서 쿼리가 효율적
		- 단일 샤드 내에서 강한 응집도
		-  다른 샤드의 entity와 연관 되는 경우 효율이 떨어짐

참고
- [쉬운코드 - Youtube](https://www.youtube.com/watch?v=P7LqaEO-nGU)
- [우아한테크 - Youtube](https://www.youtube.com/watch?v=VAhZa30j8hA)
- [우아한테크 - Youtube](https://www.youtube.com/watch?v=y42TXZKFfqQ&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=408)
- 