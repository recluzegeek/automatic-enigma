---
title: DBLAB CODE SNIPPETS
updated: 2023-02-18 03:41:18Z
created: 2023-02-18 03:35:31Z
latitude: 55.85587000
longitude: -3.16418900
altitude: 0.0000
---

## LAB-1
## LAB-2
## LAB-3
## LAB-4
## LAB-5
## LAB-6
## LAB-7
____________________________________________
- **Adding a new column in table**
	```
	alter table doctors
	add column address varchar(45);
	```

- **Renaming column**
	```
	alter table doctors
	change column address d_address varchar(30);
	```
- **Drop column**
	```
	alter table doctors
	drop column d_address;
	```
- **Changing datatype of a column**
	```
	alter table doctors
	modify column d_phone int
	```

//set default
alter table doctors
alter column d_department set DEFAULT "RE"

//drop default
alter table doctors
alter column d_department drop DEFAULT 

//updating in parent table
on-delete cascade/set null
on-update cascade/set null
__________________________________________
//retrieving data
select * from emp;

//selecting specific columns
select ename, empno , sal from emp;

1. Query to display the data of departments
select * from dept;

Arithmetic Operations
+
-
/
*
Addition
select sal,sal+100 from emp;

2.Query that shows the deduction of 10% from
salary of employee
select sal-(sal/100) from emp;

3.Display salary of employee by adding 
commision
select sal, sal+comm from emp;

//column alias
select sal, sal*0.2 as "bonus" from emp;

4.Display information of employees working
in department no 20 with department number alias.

select ename, empno,mgr, deptno "Department Number"
from emp where deptno = 20;

curdate()
sysdate()

//using literals
select "Employee", ename ," is working as" , job from emp 

//using concat function
select concat(empno,ename) as "Employee_Details" from emp;