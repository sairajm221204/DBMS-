# DBMS-
 6th
 delimiter $$
 create procedure n1(IN rno1 int)
    begin
    declare rno2 int;
    declare exit_cond boolean;
    declare c1 cursor for select rno from o_rollcall where rno>rno1;
    declare continue handler for not found set exit_cond=TRUE;
    open c1;
    l1: LOOP
    fetch c1 into rno2;
    if not exists(select * from n_rollcall where rno=rno2)then
    insert into n_rollcall select * from o_rollcall where rno=rno2;
    end if;
    if exit_cond then
    close c1;
    leave l1;
    end if;
    end loop l1;
    end;
    $$
Query OK, 0 rows affected (0.02 sec)

mysql> delimiter ;
mysql> call n1(3);


5th
delimiter $$
mysql> create procedure proc_emp()
     begin
     declare v_ename varchar(20);
     declare v_sal float;
     declare v_finished integer default 0;
     declare c1 cursor for select ename,sal from employee;
     declare continue handler for not found set v_finished=1;
     open c1;
     get_emp: LOOP
     fetch c1 into v_ename, v_sal;
     if v_finished=1 then
     leave get_emp;
     end if;
     select v_ename,v_sal;
     end LOOP get_emp;
     close c1;
   end $$

delimiter ;



4
DELIMITER $$
CREATE TRIGGER salarydiff
AFTER UPDATE on customer
FOR EACH ROW
BEGIN
DECLARE old_sal INT;
DECLARE new_sal INT;
DECLARE sal_diff INT;
SET old_sal=OLD.salary;
SET new_sal=NEW.salary;
SET sal_diff=new_sal-old_sal;
INSERT INTO salary_changes(c_id, saldiff) 
VALUES(OLD.c_id, sal_diff) ;
END $$
