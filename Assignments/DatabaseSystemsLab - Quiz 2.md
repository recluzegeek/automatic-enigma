---
title: DatabaseSystemsLab - Quiz 2
updated: 2023-04-22 03:35:43Z
created: 2023-03-28 12:33:35Z
latitude: 55.85587000
longitude: -3.16418900
altitude: 0.0000
---

## 1. Head wants to know the id and name of the project and information about the employee assigned to that project. Write a query to find the desired information.

```sql
SELECT ep.p_id, e.* from emp_proj ep 
		JOIN projects p on ep.p_id = p.P_ID
    	JOIN employees e on ep.E_ID = e.e_id;
```

## 2. Project leader wants to know the information about employee, his/her country and the project on which he/she works. As a data scientist write a query to display this information.

```sql
SELECT * from employees e
		JOIN countries c on e.c_id = c.c_id
    	JOIN emp_proj ep on e.e_id = ep.E_ID
    	JOIN projects p on ep.p_id = p.P_ID;
```

## 3. Display data of those employee working on HMS project, having salary from 2000 – 3500 and name starting with h.
```sql
SELECT * FROM employees e
		JOIN emp_proj ep ON e.e_id = ep.E_ID 
 	JOIN projects p ON ep.p_id = p.P_ID 
		WHERE p.P_name = 'HMS' AND e.e_Salary BETWEEN 2000 AND 3500 AND e.ename LIKE 'H%';

```

## 4. Display the name of those employees whose employee works in bombay.

```sql
SELECT ename FROM employees e
		JOIN countries c on e.c_id = c.c_id 
		WHERE c_city = 'Bombay';

```