##Dept table

|deptno|	dname|	loc|
|-|-|-|
|10|	Accounts|	Bangalore|
|20|	IT|	Delhi|
|30|	Production|	Chennai|
|40|	Sales|	Hyd|
|50|	Admin|	London|

```
create table dept(
    deptno int primary key,
    dname varchar(30),
    loc varchar(30)
);
```

```
insert into dept values(10,"Accounts","Bangalore");
insert into dept values(20,"IT","Delhi");
insert into dept values(30,"Production","Chennai");
insert into dept values(40,"Sales","Hyd");
insert into dept values(50,"Admin","London");
```

---

##Emp table

|empno|	ename|	sal|	hire_date|	commission|	deptno|	mgr|
|-|-|-|-|-|-|-|
|1007|	Martin|	21000|	2000-Jan-1|	1040|	null|	null|
|1003|	Stefen|	12000|	1990-Jan-1|	500|	20|	1007|
|1006|	Dravid|	19000|	1985-Jan-1|	2400|	10|	1007|
|1001|	Sachin|	19000|	1980-Jan-1|	2100|	20|	1003|
|1002|	Kapil|	15000|	1970-Jan-1|	2300|	10|	1003|
|1004|	Williams|	9000|	2001-Jan-1|	null|	30|	1007|
|1005|	John|	5000|	2005-Jan-1|	null|	30|	1006|

```
create table emp(
        empno int primary key,
        ename varchar(40),
        sal int,
        hire_date date,
        commission int,
        deptno int,
        mgr int,
        foreign key(deptno) references dept(deptno)
    );
```

```
alter table emp add foreign key (mgr) references emp(empno);
```

```
insert into emp values(1007,"Martin",21000,"2000-Jan-1",1040,NULL,NULL);
insert into emp values(1003,"Stefen",12000,"1990-Jan-1",500,20,1007);
insert into emp values(1006,"Dravid",19000,"1985-Jan-1",2400,10,1007);
insert into emp values(1001,"Sachin",19000,"1980-Jan-1",2100,20,1003);
insert into emp values(1002,"Kapil",15000,"1970-Jan-1",2300,10,1003);
insert into emp values(1004,"Williams",9000,"2001-Jan-1",NULL,30,1007);
insert into emp values(1005,"John",5000,"2005-Jan-1",NULL,30,1006);
```

---
##Queries

1. Select employee details  of dept number 10 or 30
    ```
    select * from emp where deptno in (10,30)
    ```

2. Write a query to fetch all the dept details with more than 1 Employee.
    ```
    select * from dept where deptno in (select deptno from emp group by deptno having count()(deptno)>1);
    ```

3. Write a query to fetch employee details whose name starts with the letter “S”
    ```
    select * from emp where ename like "s%";
    ```

4. Select Emp Details Whose experience is more than 2 years
    ```
    select * from emp where datediff(now(),hire_date)/365>2;
    ```

5. Write a SELECT statement to replace the char “a” with “#” in Employee Name ( Ex:  Sachin as S#chin)
    ```
    select replace(ename,"a","#") from emp;
    ```

6. Write a query to fetch employee name and his/her manager name.
    ```
   select e.ename, m.ename from emp e,emp m where e.mgr=m.empno;
    ```

7. Fetch Dept Name , Total Salry of the Dept
    ```
    select d.dname, ifnull(sum(e.sal),0) as totalSal from dept d left join emp e on d.deptno=e.deptno group by dname;
    ```

8. Write a query to fetch ALL the  employee details along with department name, department location, irrespective of employee existance in the department.
    ```
    select * from emp e left join dept d on e.deptno=d.deptno;
    ```

9. Write an update statement to increase the employee salary by 10 %
    ```
    update emp set sal=sal+(sal*10/100);
    ```

10. Write a statement to delete employees belong to Chennai location.
    ```
    delete from emp where deptno in(select deptno from dep where loc like "chennai");
    ```

11. Get Employee Name and gross salary (sal + comission).
    ```
    select ename, (sal+ifnull(commission,0))as grossSal from emp;
    ```

12. Increase the data length of the column Ename of Emp table from  100 to 250 using ALTER statement
    ```
    alter table emp modify update ename varchar(250); 
    ```

13. Write query to get current datetime
    ```
    select now();
    ```

14. Write a statement to create STUDENT table, with related 5 columns
    ```
    create table student(
        usn varchar(10) primary key,
        name varchar(40),
        semester int,
        cgpa int,
        branch varchar(30)
    );
    ```

15. Write a query to fetch number of employees in who is getting salary more than 10000
    ```
    select count(sal) from emp where sal>=10000;
    ```

16. Write a query to fetch minimum salary, maximum salary and average salary from emp table.
    ```
    select min(sal), max(sal), avg(sal) from emp;
    ```

17. Write a query to fetch number of employees in each location
    ```
    select d.loc, count(e.empno) from dept d, emp e where d.deptno=e.deptno group by d.loc;
    ```

18. Write a query to display emplyee names in descending order
    ```
    select ename from emp order by ename desc;
    ```

19. Write a statement to create a new table(EMP_BKP) from the existing EMP table
    ```
    create table emp_bkp select * from emp;
    ```

20. Write a query to fetch first 3 characters from employee name appended with salary.
    ```
    select concat(left(ename,3),sal) from emp;
    ```
 
21. Get the details of the employees whose name starts with S
    ```
    select * from emp where ename like "s%";
    ```

22. Get the details of the employees who works in Bangalore location
    ```
    select * from emp where deptno in(select deptno from dept where loc="bangalore");
    ```

23. Write the query to get the employee details whose name started within  any letter between  A and K
    ```
    select * from emp where ename regexp "^[a-k]";
    ```

24. Write a query in SQL to display the employees whose manager name is Stefen 
    ```
    select * from emp where mgr in (select empno from emp where ename="stefen");
    ```

25. Write a query in SQL to list the name of the managers who is having maximum number of employees working under him 
    ```
    select ename as Manager from emp where empno=(select mgr from emp group by mgr order by count(mgr) desc limit 1);
    ```

26. Write a query to display the employee details, department details and the manager details of the employee who has second highest salary
    ```
    select * from emp where empno in (select empno from emp where sal=(select distinct sal from emp order by sal desc limit 1,1));
    ```

27. Write a query to list all details of all the managers
    ```
    select e.empno, e.ename, e.hire_date, e.deptno, d.dname, d.loc from emp e, dept d where e.deptno=d.deptno and empno in (select distinct mgr from emp);
    ```


28. Write a query to list the details and total experience of all the managers
    ```
    select empno, ename, deptno, round(datediff(now(),hire_date)/365,0) as "experience(years)"  from emp where empno in (select distinct mgr from emp);
    ```

29. Write a query to list the employees who is manager and  takes commission less than 1000 and works in Delhi
    ```
    select * from emp e join dept d on e.deptno=d.deptno where d.loc="delhi" and e.commission<1000 and e.empno in (select distinct mgr from emp);
    ```

30. Write a query to display the details of employees who are senior to Martin
    ```
    select * from emp where year(hire_date)<(select year(hire_date) from emp where ename="martin");
    ```