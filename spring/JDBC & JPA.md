## JDBC
### 연결 설정
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

