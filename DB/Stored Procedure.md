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
```