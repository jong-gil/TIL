- 백엔드 서버와 DB 서버 통신 시 TCP 기반으로 동작
	- 쿼리 요청 시 DB에 접근할 때 매번 connection을 열고 닫아야 함 -> 시간적 비용 발생
		- open connection 시 3-way handshake
		- close connection 시 4-way handshake

- Connection Pool Data Sources
	- 데이터 소스의 개념 및 역할과 유사함
	- 일반적인 connection 인스턴스 대신 *pooled connection 인스턴스들* 을 리턴함
- Pooled Connections
	- pooled connection 인스턴스는 데이터베이스와 연결된 하나의 물리적 연결을 나타냄 (1:1 대응)
	- 일련의 논리적 연결 인스턴스들이 사용하고 있을 때 계속 열려있음

### 개념
- connection pooling을 사용하면 물리적인 데이터베이스 연결들이 여러 개의 논리적 연결 인스턴스에 의해 재사용 될 수 있음
![[dbcp.png]]

### Hikari CP
- JDBC 기반 Connection Pool
- `miminumIdle`
	- idle connection의 수가 이 값보다 작고, 총 connection의 수가 `maximumPoolSize`보다 작다면, 추가적인 connection을 만듦
	- 하지만, 효율적 운영을 위해 default 값인, `maximumPoolSize`와 동일한 값으로 세팅된 것을 사용하기를 권장
- `maximumPoolsize`
	- idle (대기) 상태와 in-use(활성화) 상태인 connection을 합친 풀의 최댓값을 제어함
	- DB 백엔드와 연결된 실제 connection의 최댓값을 결정
	- pool이 해당 사이즈에 도달하고, idle connection이 없을 때, getConnection() 호출이 `connectionTimeout` 만큼 제한됨
	- 기본값: 10
- `maxLifeTime`
	- pool 에서 connection의 최대 수명
	- in-use 인 상태에서는 적용 안됨 -> close 된 상태 이후에 제거됨
	- DB 혹인 인프라가 적용하는 connection time limit (wait_timeout) 보다 몇 초 짧게 설정해야 함 
	- 기본값: 1800000 (30분)
- `connectionTimeout`
	- pool에서 connection을 받기 위한 대기 시간
	- 이 시간이 지나도 connection이 가능하지 않으면 SQLException 발생
	- 최솟값: 250ms, 최댓값: 30000 (30초)


참고
[Youtube - 쉬운코드](https://www.youtube.com/watch?v=zowzVqx3MQ4)
[HikariCP Github](https://github.com/brettwooldridge/HikariCP?tab=readme-ov-file)
