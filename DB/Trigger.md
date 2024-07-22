```MySql
delimiter $$
CREATE TRIGGER log_user_nickname_trigger
BEFORE UPDATE
ON users FOR EACH ROW
BEGIN
	insert into users_log values(OLD.id, OLD.nickname, now());
END
$$
delimiter ;
```
	-OLD: UPDATE 되기 전의 tuple (DELETE라면 delete된 tuple)
