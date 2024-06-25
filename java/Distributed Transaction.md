> *global transaction* 이라고도 불리며 두 개 이상의 연관된 트랜젝션이 관리되어야 할 때 사용됨

- 일반적으로 JDBC transaction은 local함
- 하나의 connection이 한 번에 하나의 transaction만 처리 가능
- transaction 처리가 끝나면, 영속화를 위해 커밋이나 롤백을 호출함

### JTA
> Java Transaction API

- transaction을 Connection 객체와 분리(decouple)할 수 있는 기능을 제공
- 하나의 connection이 동시에 여러 개의 transaction을 동시에 처리할 수 있게 함
- 하나의 transaction에서 여러 개의 connection을 처리할 수 있게 함

출처
[IBM Docs](https://www.ibm.com/docs/en/i/7.5?topic=driver-jdbc-distributed-transactions)
