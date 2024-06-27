## JDBC
#### 연결 설정
- JDBC 어플리케이션은 다음 두 클래스들 중 하나를 타겟으로 데이터 소스를 연결함
- `DriverManager`
	- 데이터베이스 URL로 특정된 데이터 소스에 연결
	- 샘플코드
```java
public Connection getConnection() throws SQLException {

    Connection conn = null;
    Properties connectionProps = new Properties();
    connectionProps.put("user", this.userName);
    connectionProps.put("password", this.password);

    if (this.dbms.equals("mysql")) {
        conn = DriverManager.getConnection(
                   "jdbc:" + this.dbms + "://" +
                   this.serverName +
                   ":" + this.portNumber + "/",
                   connectionProps);
    } else if (this.dbms.equals("derby")) {
        conn = DriverManager.getConnection(
                   "jdbc:" + this.dbms + ":" +
                   this.dbName +
                   ";create=true",
                   connectionProps);
    }
    System.out.println("Connected to database");
    return conn;
}
```
		- MySQL: jdbc://mysql://localhost:3306/
		- Java DB: jdbc:drby:testdb;create=true
			- testdb: 데이터베이스 이름
			- create=true: DBMS가 데이터베이스를 생성하라는 지시
- `DataSource`
	- 데이터 소스가 가지고 있는 상세한 정보를 표시
	- `DataSource` 객체의 특성들이 특정한 데이터 소스를 나타냄
	- `DriverManager`보다 선호되는 방법
	- [[Distributed Transaction]]을 지원하는 전형적인 `DataSource` 클래스는 [[DBCP]]도 지원함
	- `DataSource`클래스 인스턴스 생성 및 특성 지정
	- 샘플코드
```java
// 연결 설정
OracleDataSource ods = new OracleDataSource();
ods.setDriverType("oci");
ods.setServerName("dlsun999");
ods.setNetworkProtocol("tcp");
ods.setDatabaseName("816");
ods.setPortNumber(1521);
ods.setUser("scott");
ods.setPassword("tiger");

// JNDI를 통한 데이터 소스 인스턴스 등록
Context ctx = new InitialContext();
ctx.bind("jdbc/sampledb", ods);

// 연결하기
OracleDataSource odsconn = (OracleDataSource)ctx.lookup("jdbc/sampledb");
Connection conn = odsconn.getConnection();
```
<details>
<summary>JNDI</summary>

- JNDI (Java Naming and Directory Interface)는 어플리케이션이 원격 서비스나 자원들 찾는 방법
- 모든 JDBC 데이터 소스들은 JNDI가 참조할 수 있음
- JNDI 기능을 사용하기 위해서는 `jndi.jar` 파일이 CLASSPATH에 존재해야 함
</details>

## JPA
> Java Persistence API

- Spring 의존성 추가 없이 `PersistenceAnnotationBeanPostProcessor`가 활성화 되어 있다면 `@PersistencUnit`과 `@PersistenceContext` 인식 가능

```java
public class ProductDaoImpl implements ProductDao {
	// EntityManagerFactory 객체
	private EntityManagerFactory emf;

	@PersistenceUnit
	public void setEntityManagerFactory(EntityManagerFactory emf) {
		this.emf = emf;
	}

	public Collection loadProductsByCategory(String category) {
		// EntityManagerFactory -> EntityManager instace 생성
		EntityManager em = this.emf.createEntityManager();
		try {
			Query query = em.createQuery("from Product as p where p.category = ?1");
			query.setParameter(1, category);
			return query.getResultList();
		}
		finally {
			if (em != null) {
				em.close();
			}
		}
	}
}
```

#### EntityManagerFactory
- `EntityManager`인스턴스를 생성하는 역할
- 어플리케이션이 시작될 때, DB당 하나 생성됨
- thread-safe
- PersistenceUnit의 상세 정보에 의해 정의됨 (주입됨)
- DB 연결 상세정보, entity 매핑 정보 등의 설정을 가지고 있음


#### EntityManger
- `EntityManagerFactory`에 의해 생성되는 것이 원칙
- persistence context와 상호작용하면서 DB 연산을 수행 (EntityManager를 주입하는 역할)
- Persistence Context
	- 관리되고 있는 entity들의 집합을 나타냄
	- 메모리에 올라가 있고 변화가 추적되는 entity
- Entity Lifecycle을 관리
	1. New (Transient)
		- `new`연산자로 생성되었으나, `EntityManager`와 연결되지 않은 상태
		- persistence context와 연관되지 않고, DB의 row와도 상응하지 않음
	2. Managed
		- persistence context와 연관되고 EntityManager에 의해 추적되는 상태
		- `em.persist(entity)`, `em.find(entityClass, primaryKey)`
	3. Detached
		- 이전에 관리되었으나 더 이상 persistence context와 연관이 없는 상태
		- persistence context가 종료되거나
		- `em.detach(entity)`, `em.clear()`, `em.close()`
	4. Removed
		- entity가 DB에서 제거되기로 예정된 상태
		- transaction commit 될 때까지 관리됨
		- `em.remove(entity)`

<details>
<summary>참고사항</summary>

상태변화: Detached -> Managed: `em.merge(entity)`
</details>

> 추가 연산
> - em.refresh(): 데이터베이스로부터 모든 상태를 reload
> - em.flush(): persistence context를 데이터베이스와 동기화

- Transaction 관리: scope에 있는 transaction 연산 관리
- `EntityManager` 단독으로 생성될 수 있음
	- shared EntityManager라고 불림
	- thread-safe 한 proxy를 호출
	- 예시 코드
```java
public class ProductDaoImpl implements ProductDao {

	@PersistenceContext
	private EntityManager em;

	public Collection loadProductsByCategory(String category) {
		Query query = em.createQuery("from Product as p where p.category = :category");
		query.setParameter("category", category);
		return query.getResultList();
	}
}
```
