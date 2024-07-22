- 사용자가 정의한 함수
- DBMS에 저장되고 사용되는 함수
- SQL의 `select, insert, update, delete` statement에서 사용 가능

#### 예제 1
- ID를 10자리 정수로 랜덤하게 발급
- ID의 앞자리는 1로 고정
```mysql
# semi-colon(;)이 function의 마지막이라고 인식하지 않게 함
delimiter $$
CREATE FUNCTION id_generator()
# return type
RETURNS int
# create의 문법과 상관없이 mysql의 문법
NO SQL
# body (BEGIN으로 시작 END로 종료)
BEGIN
	RETURN (1000000000 + floor(rand() * 100000000));
END
$$
delimiter ;
```
Employee Table

| id  | name | birth_date | sex | position | salary | dept_id |
| --- | ---- | ---------- | --- | -------- | ------ | ------- |
```mysql
INSERT INTO employee
VALUES (id_generator(), 'JEHN', '1991-08-01', 'F', 'PO', 10000, 1005)
```

#### 예제 2
- 부서의 ID를 파라미터로 받아서 해당 부서의 평균 연봉을 알려주는 함수
```mysql
delimiter $$
CREATE FUNCTION dept_avg_salary(d_id int)
RETURNS int
READS SQL DATA
BEGIN
	DECLARE avg_sal int;
	select avg(salary) into avg_sal # (@avg_sal)
		from employee
		where dept_id = d_id;
	RETURN avg_sal; # (@avg_sal)
END
$$
delimiter ;
```

Department Table

| id  | name | leader_id |
| --- | ---- | --------- |

```mysql
SELECT *, dept_avg_salary(id)
FROM department;
```

#### 예제 3
- 토익 800 이상을 충족했는지 알려주는 함수
```mysql
delimiter $$
CREATE FUNCTION toeic_pass_fail(toeic_score int)
RETURNS char(4)
NO SQL
BEGIN
	DECLARE pass_fail char(4)
	IF     toeic_score is null THEN SET pass_fail = 'fail';
	ELSEIF toeic_score < 800 THEN SET pass_fail = 'fail';
	ELSE                          SET pass_fail = 'pass';
	END IF;
	RETURN pass_fail;
END
$$
delimiter ;
```

```mysql
SELECT *, toeic_pass_fail(toeic)
FROM student;
```
> stored function 삭제 시 `DROP FUCNTION <function_name>;`
### 특징
- loop을 돌면서 반복적 작업 수행
- `case` keyword 사용하여 값에 따라 분기 처리
- 에러 핸들링, 에러 일으키기 등 다양한 동작 정의 가능

#### 등록된 stored function 파악
```mysql
# 등록된 stored function 목록 조회
SHOW FUNCTION STATUS where DB = 'company';

# 해당 stored function의 코드 확인
SHOW CREATE FUNCTION id_generator;
```
#### 언제 쓰면 좋은가
##### Three-tier Architecture
- Presentation tier
	- 사용자에게 보여지는 부분을 담당
	- HTML, javascript, CSS, native app, desktop app, ...
- Logic tier
	- 실제 서비스와 관련된 기능, 정책 등 비지니스 로직을 담당
	- application tier, middle tier 라고도 불림
	- Java + Spring, Python + Django, ...
- Data tier
	- 데이터를 저장, 관리, 제공하는 역할
	- MySQL, Oracle, SQL Server, PostgreSql, MongoDB, ...
##### 제안
- util 함수로써의 기능
- 비지니스 로직이 포함된다면 부적절
	-> logic tier의 기능을 data tier가 가지는 것 -> 관리 비용 증대
- 예제 1, 2, 3
