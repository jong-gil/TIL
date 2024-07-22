- 사용자 정의 프르시저
- RDBMS에 저장되고 사용되는 프로시저
- 구체적인 하나의 테스크 수행

#### 예제 1
- 두 정수의 곱셈 결과를 가져오기
```mysql
delimiter $$
CREATE PROCEDURE product(IN a int, IN b int, OUT result int)
BEGIN
	SET result = a * b;
END
delimiter ;

# product 호출
# @result -> 사용자 정의 변수
call product(5, 7, @result);
select @result;
```
#### 예제 2
- 두 정수를 맞바꾸기 (스왑)
```mysql
delimiter $$
CREATE PROCEDURE swap(INOUT a int, INOUT b int)
BEGIN
	set @temp = a;
	set a = b;
	set b = @temp;
END
$$
delimiter ;

set @a = 5, @b = 7;
call swap(@a, @b);
select @a, @b;
```
	- INOUT keyword

#### 예제 3
- 각 부서별 평균 연봉
```mysql
delimiter $$
CREATE PROCEDURE dept_avg_salary()
BEGIN
	select dept_id, avg(salary)
	from employee
	group by dept_id;
END
$$
delimiter ;

call dept_avg_salary();
```

#### 예제 4
- 사용자가 프로필 닉네임 변경 -> 이전 닉네임 로그에 저장 && 새 닉네임으로 업데이트
**Users Table**

| id  | nickname |
| --- | -------- |


**Nickname_logs Table**

| id  | prev_nickname | until |
| --- | ------------- | ----- |

```mysql
delimiter $$
CREATE PROCEDURE change_nickname(user_id INT, new_nick varchar(30))
BEGIN
	insert into nickname_logs (
		select id, nickname, now() from users where id = user_id
	);
	update users set nickname = new_nick where id = user_id;
END
$$
delimiter ;

call change_nickname(1, 'ZIDANE');
```
#### Stored Function과의 차이점
- `return` 키워드 사용 불가 
	- SQL server의 경우 상태코드 반환용으로 사용 가능 (INT 타입)
- 파라미터로 값 반환 가능
	- stored function은 일부 가능함 - postgreSQL의 경우 가능함
- statement 안에서 사용 불가능
- transaction 사용 가능
- 주된 사용 목적
	- business logic
	- stored function은 복잡한 계산을 할 때 사용

#### 장점
- application에 transparent함
	- 여러 개의 application 인스턴스들이 DB를 참조하고 있을 때
	- 로직 상의 변경이 발생하면 인스턴스 하나 씩 재기동을 해야함
	- (로직의 이름과 파라미터가 그대로라면)DB만 재기동 하면 됨
- network traffic을 줄여 응답 속도를 향상시킬 수 있음
	- 로직 상 `select, insert, update` 등의 statement가 있을 때 각 명령 마다 DB와 통신해야 함
	- procedure로 관리 된다면 한번의 호출로 처리 가능
- 여러 서비스에서 재사용 가능
	- 여러 서비스(Spring, Django, Node 등)가 하나의 DB를 참조하고 있다면
	- 다양한 언어로 개발하지 않고 하나의 procedure만 호출하면 됨
- 민감한 정보에 대한 접근을 제한할 수 있음
	- 정보에 대한 직접적 접근보다 procedure에 대한 접근만 허용

#### 단점
- 유지 관리 보수 비용이 커짐
	- 제대로된 로직 관리가 힘듦
	- logic - procedure 버전/코드 관리를 동시에 해야함
- DB 서버 추가의 복잡성
	- RDBMS의 CPU 사용량 증가 (서비스:DB = N:1)
	- 급작스러운 트래픽 증가 시 DB에 부하 증가
	- DB 서버 증설 시 기존 DB 정보를 모두 가져와야 함
- 언제나 transparent한 것은 아님
	- procedure의 이름 및 파라미터 변경 시 application 단에서 코드를 수정해야 함
- transparent 하다고 무조건 좋은 것은 아님
	- 예상치 못한 문제의 영향이 전파됨
	- application 단에서 관리했다면 하나의 인스턴스의 문제로 제한할 수 있음
- 비지니스 로직을 소스코드에 두고도 응답속도 향상이 가능함
	-  `select, update`등을 동시에 진행이 가능할 때 (순차적일 필요가 없을 때)
		- thread pool, Non-block I/O 등 사용 -> DB에 동시에 요청
	- 캐시 사용
- 복잡하고 유연한 코드 작성이 어려움
- 가독성이 떨어짐
- 디버깅이 어려움

---
참고
- [쉬운코드 - Youtube](https://www.youtube.com/watch?v=SOLm-GXFzG8&list=PLcXyemr8ZeoT-_8yBc_p_lVwRRqUaN8ET&index=47)