# SQL-Practice

Here is a little practice in SQL. 
The following questions and queries were created and answered using PostgreSQL through pdAdmin 4.

**Creating Sample Data**
```

CREATE TABLE Worker (
	WORKER_ID SERIAL PRIMARY KEY,
	FIRST_NAME CHAR(25),
	LAST_NAME CHAR(25),
	SALARY INTEGER,
	JOINING_DATE TIMESTAMP,
	DEPARTMENT CHAR(25)
);

INSERT INTO Worker 
	(WORKER_ID, FIRST_NAME, LAST_NAME, SALARY, JOINING_DATE, DEPARTMENT) VALUES
		(001, 'Monika', 'Arora', 100000, '2014-02-20 09:00:00', 'HR'),
		(002, 'Niharika', 'Verma', 80000, '2014-06-11 09:00:00', 'Admin'),
		(003, 'Vishal', 'Singhal', 300000, '2014-02-20 09:00:00', 'HR'),
		(004, 'Amitabh', 'Singh', 500000, '2014-02-20 09:00:00', 'Admin'),
		(005, 'Vivek', 'Bhati', 500000, '2014-06-11 09:00:00', 'Admin'),
		(006, 'Vipul', 'Diwan', 200000, '2014-06-11 09:00:00', 'Account'),
		(007, 'Satish', 'Kumar', 75000, '2014-01-20 09:00:00', 'Account'),
		(008, 'Geetika', 'Chauhan', 90000, '2014-04-11 09:00:00', 'Admin');

CREATE TABLE Bonus (
	WORKER_REF_ID INT,
	BONUS_AMOUNT INTEGER,
	BONUS_DATE TIMESTAMP,
	FOREIGN KEY (WORKER_REF_ID)
		REFERENCES Worker(WORKER_ID)
        ON DELETE CASCADE
);

INSERT INTO Bonus 
	(WORKER_REF_ID, BONUS_AMOUNT, BONUS_DATE) VALUES
		(001, 5000, '2016-02-20'),
		(002, 3000, '2016-06-11'),
		(003, 4000, '2016-02-20'),
		(001, 4500, '2016-02-20'),
		(002, 3500, '2016-06-11');
		
CREATE TABLE Title (
	WORKER_REF_ID INT,
	WORKER_TITLE VARCHAR(25),
	AFFECTED_FROM TIMESTAMP,
	FOREIGN KEY (WORKER_REF_ID)
		REFERENCES Worker(WORKER_ID)
        ON DELETE CASCADE
);

INSERT INTO Title 
	(WORKER_REF_ID, WORKER_TITLE, AFFECTED_FROM) VALUES
 (001, 'Manager', '2016-02-20 00:00:00'),
 (002, 'Executive', '2016-06-11 00:00:00'),
 (008, 'Executive', '2016-06-11 00:00:00'),
 (005, 'Manager', '2016-06-11 00:00:00'),
 (004, 'Asst. Manager', '2016-06-11 00:00:00'),
 (007, 'Executive', '2016-06-11 00:00:00'),
 (006, 'Lead', '2016-06-11 00:00:00'),
 (003, 'Lead', '2016-06-11 00:00:00');

```

## Q-1. Write an SQL query to fetch “FIRST_NAME” from Worker table using the alias name as <WORKER_NAME>.
```
SELECT first_name AS worker_name FROM Worker ;
```

## Q-2. Write an SQL query to fetch “FIRST_NAME” from Worker table in upper case.
```
SELECT UPPER(first_name) FROM Worker ;
```

## Q-3. Write an SQL query to fetch unique values of DEPARTMENT from Worker table.
```
SELECT DISTINCT(department) FROM Worker ;
```

## Q-4. Write an SQL query to print the first three characters of  FIRST_NAME from Worker table.
```
SELECT substring(first_name,1,3) FROM Worker ;
```

## Q-5. Write an SQL query to find the position of the alphabet (‘a’) in the first name column ‘Amitabh’ from Worker table.
```
SELECT first_name, POSITION('a' IN first_name )
FROM Worker WHERE first_name = 'Amitabh' ;

```
## Extra1 - Select the first name, last name and the position of the substring 'an' within last_name for those rows only where the substirng exists from the worker table.
```
SELECT first_name,last_name, POSITION('a' IN last_name )
FROM Worker WHERE POSITION('an' IN last_name)>0 ;
```

## Q-6. Write an SQL query to print the FIRST_NAME from Worker table after removing white spaces from the right side.
```
SELECT first_name, RTRIM(first_name) FROM Worker  ;
```

## Q-7. Write an SQL query to print the DEPARTMENT from Worker table after removing white spaces from the left side.
```
SELECT department, LTRIM(department) FROM Worker  ;
```

## Q-8. Write an SQL query that fetches the unique values of DEPARTMENT from Worker table and prints its length.
```
SELECT DISTINCT(LENGTH(department)) FROM Worker  ;
```

## Q-9. Write an SQL query to print the FIRST_NAME from Worker table after replacing ‘a’ with ‘A’.
```
SELECT first_name, REPLACE(first_name, 'a','A') FROM Worker ;
```

## Q-10. Write an SQL query to print the FIRST_NAME and LAST_NAME from Worker table into a single column COMPLETE_NAME. A space char should separate them.
```
SELECT first_name || ' '||last_name AS complete_name FROM Worker ;

OR

SELECT CONCAT(first_name,'',last_name) AS Complete_Name FROM Worker ;
```

## Q-11. Write an SQL query to print all Worker details from the Worker table order by FIRST_NAME Ascending
```
SELECT * FROM Worker ORDER BY first_name ASC ;
```


## Q-12. Write an SQL query to print all Worker details from the Worker table order by FIRST_NAME Ascending and DEPARTMENT Descending.
```
SELECT first_name, department FROM worker 
ORDER BY first_name,department DESC;
```

## Q-13. Write an SQL query to print details for Workers with the first name as “Vipul” and “Satish” from Worker table.
```
SELECT * FROM worker 
WHERE first_name = 'Vipul' OR first_name = 'Satish'

OR

SELECT * FROM worker 
WHERE first_name IN ('Vipul','Satish')

```

## Q-14. Write an SQL query to print details of workers excluding first names, “Vipul” and “Satish” from Worker table.
```
SELECT * FROM worker 
WHERE first_name NOT IN ('Vipul','Satish')
```

## Q-15. Write an SQL query to print details of Workers with DEPARTMENT name as “Admin”.
```
SELECT * FROM worker 
WHERE department IN ('Admin')

OR

SELECT * FROM worker 
WHERE department ILIKE '%admin%';

```

## Q-16. Write an SQL query to print details of the Workers whose FIRST_NAME contains ‘a’.
```
SELECT * FROM worker 
WHERE first_name ILIKE '%a%';
```

## Q-17. Write an SQL query to print details of the Workers whose FIRST_NAME ends with ‘a’.
```
SELECT * FROM worker 
WHERE first_name ILIKE '%a';
```

## Q-18. Write an SQL query to print details of the Workers whose FIRST_NAME ends with ‘h’ and contains six alphabets.
```
SELECT * FROM worker 
WHERE first_name ILIKE '_____h';
```
## Q-19. Write an SQL query to print details of the Workers whose SALARY lies between 100000 and 500000.
```
SELECT * FROM worker 
WHERE salary Between 100000 AND 500000 ;
```

## Q-20. Write an SQL query to print details of the Workers who have joined in Feb’2014.
```
SELECT * FROM worker 
WHERE joining_date BETWEEN '2014-02-01' AND '2014-03-01' 
```

## Extra2 - Write a SQL query to get the second highest salary from the worker table.
```
SELECT MAX(salary) AS Second_highest_salary FROM worker
WHERE salary < (SELECT MAX(salary) FROM WORKER)
 ;
 
 OR (more specific)
 
SELECT DISTINCT(salary) FROM worker
ORDER BY salary DESC LIMIT 1 OFFSET 1
 ;
 
 
```

## Q-21. Write an SQL query to fetch the count of employees working in the department ‘Admin’.
```
SELECT COUNT(*)
FROM worker WHERE DEPARTMENT = 'Admin' ;
```

## Q-22. Write an SQL query to fetch worker names with salaries >= 50000 and <= 100000.
```
SELECT first_name ||' '|| last_name AS Workers , salary
FROM worker WHERE salary BETWEEN 50000 AND 100000 ;
```

## Q-23. Write an SQL query to fetch the no. of workers for each department in the descending order.
```
SELECT department, COUNT(*) AS num_of_workers
FROM worker 
GROUP BY department
ORDER BY num_of_workers DESC
;
```

## Q-24. Write an SQL query to print details of the Workers who are also Managers.
```
SELECT first_name, last_name
FROM worker
INNER JOIN title ON
worker.worker_id = title.worker_ref_id
WHERE worker_title = 'Manager'
;
```

## Q-25. Write an SQL query to fetch duplicate records having matching data in some fields of a table.
```
-- I checked and the only table to fit this was title
SELECT worker_title, affected_from, COUNT(*) FROM title
GROUP BY worker_title,affected_from
HAVING COUNT(*) > 1
;
```

## Q-26. Write an SQL query to show only odd rows from a table.
```
SELECT * FROM worker
WHERE MOD(worker_id,2) = 1 ;
```

## Q-27. Write an SQL query to show only even rows from a table.
```
SELECT * FROM worker
WHERE MOD(worker_id,2) = 0 ;
```

## Q-28. Write an SQL query to clone a new table from another table.
```
-- With data
CREATE TABLE clone_worker AS 
TABLE worker;

OR 

-- with no data
CREATE TABLE clone_worker AS 
TABLE worker
WITH NO DATA;
```

## Q-29. Write an SQL query to fetch intersecting records of two tables.
```
SELECT * FROM Worker
INTERSECT
SELECT * FROM clone_worker ;
```

## Q-30. Write an SQL query to show records from one table that another table does not have.
```
-- not sure about this one
SELECT *
FROM worker
LEFT JOIN title ON worker.worker_id = title.worker_ref_id
WHERE title.worker_ref_id IS NULL ;

OR

SELECT *
FROM worker
WHERE NOT EXISTS 
    (SELECT * 
     FROM title
     WHERE title.worker_ref_id = worker.worker_id) ;
```

## Q-31. Write an SQL query to show the current date and time.
```
--just the date
SELECT Current_Date;

OR
-- timestamp with zone
SELECT NOW();

OR

-- timestamp with zone
SELECT CURRENT_TIMESTAMP;
```

## Q-32. Write an SQL query to show the top n (say 10) records of a table.
```
SELECT * FROM worker
ORDER BY salary DESC
LIMIT 10 ;
```

## Q-33. Write an SQL query to determine the nth (say n=5) highest salary from a table.
```
SELECT DISTINCT(salary)
FROM worker
ORDER BY salary DESC
LIMIT 1
OFFSET 4; 
```

## Q-34. Write an SQL query to determine the 5th highest salary without using TOP or limit method.
```
-- revirew carefully
SELECT *
FROM worker w1
WHERE 4 =(
	SELECT COUNT(DISTINCT(w2.salary))
	FROM worker w2
	WHERE w2.salary > w1.salary
)
;
```

## Q-35. Write an SQL query to fetch the list of employees with the same salary.
```
-- read carefully
SELECT w1.worker_id, w1.first_name, w1.last_name, w1.salary, w1.department
FROM worker w1, worker w2
WHERE w1.salary = w2.salary AND
      w1.worker_id != w2.worker_id
;
```

## Q-36. Write an SQL query to show the second highest salary from a table.
```
SELECT DISTINCT(salary)
FROM worker
ORDER BY salary DESC
LIMIT 1 OFFSET 1
;

OR

SELECT MAX(salary) FROM worker 
WHERE salary NOT IN (SELECT MAX(salary) FROM worker);

```

## Q-37. Write an SQL query to show one row twice in results from a table.
```
SELECT * 
FROM worker w1 WHERE w1.worker_id =1
UNION ALL
SELECT * FROM worker w2 WHERE w2.worker_id =1
;

```

## Q-38. Write an SQL query to fetch intersecting records of two tables.
```
SELECT * FROM worker 
INNER JOIN bonus ON
worker.worker_id = worker_ref_id
;
```
## Extra 3 - Return a table that compares worker salary based on the same department.
```
-- A bit confusing, need to retry
SELECT  w1.first_name ||' '|| w1.last_name AS Name,
w2.first_name ||' '|| w2.last_name AS Name, w1.salary, w2.department 
FROM worker as w1
INNER JOIN worker AS w2 ON 
w1.worker_id != w2.worker_id
AND w1.department = w2.department
ORDER BY department
;
```

## Q-39. Write an SQL query to fetch the first 50% records from a table. 
```
SELECT * FROM worker 
WHERE worker_id <= (SELECT COUNT(*)/2 FROM worker) ;
```

## Q-40. Write an SQL query to fetch the departments and their number of workers.
```
SELECT department, COUNT(*) AS Worker_count FROM worker
GROUP BY department ;
```

## Extra 4 -Write an SQL query to fetch the departments with less than 3 workers.
```
SELECT department, COUNT(*) AS Worker_count FROM worker
GROUP BY department 
HAVING COUNT(*) < 3
;
```
## Q-41. Write an SQL query to show all departments along with the number of people in there.
```
SELECT department, COUNT(department) AS Worker_count FROM worker
GROUP BY department ;
```

## Q-42. Write an SQL query to show the last record from a table.
```
SELECT *  FROM worker
Order  BY worker_id DESC LIMIT 1;

OR

SELECT * FROM worker 
WHERE worker_id = (SELECT MIN(worker_id) FROM worker);
```

## Q-43. Write an SQL query to fetch the first row of a table.
```
SELECT *  FROM worker
Order  BY worker_id  LIMIT 1;

OR

SELECT * FROM worker 
WHERE worker_id = (SELECT MIN(worker_id) FROM worker);
```

## Q-44. Write an SQL query to fetch the last five records from a table.
```
SELECT * FROM worker 
ORDER BY worker_id DESC
LIMIT 5 ;
```

## Q-45. Write an SQL query to print the name of employees having the highest salary in each department.
```
-- this returns the highest salary from each department
SELECT DISTINCT(department), MAX(salary) AS Highest_Sal
FROM Worker
GROUP BY (department)
;


-- works but it returns two persons with the same salary from the same department
SELECT worker.first_name, worker.salary, worker.department
FROM worker
INNER JOIN (SELECT  MAX(DISTINCT(salary)) AS Highest, department FROM worker GROUP BY department) AS b
ON worker.department = b.department AND worker.salary = b.Highest
;

```

## Q-46. Write an SQL query to fetch three max salaries from a table.
```
-- this returns the 3 highest salaries with all other info 
SELECT * FROM worker
ORDER BY SALARY DESC
LIMIT 3
;

OR

-- this returns the top 3 unique salaries
SELECT DISTINCT(salary) FROM worker
ORDER BY SALARY DESC
LIMIT 3
;

```

## Q-47. Write an SQL query to fetch three min salaries from a table
```
SELECT DISTINCT(salary) FROM worker
ORDER BY SALARY 
LIMIT 3
```

## Q-48. Write an SQL query to fetch nth max salaries from a table.
```
-- n is the desired salary position
SELECT  DISTINCT(salary) FROM worker AS w1 
WHERE n >= (SELECT COUNT(DISTINCT salary) FROM worker AS w2 
			WHERE w1.salary <= w2.salary) 
ORDER BY w1.salary DESC
LIMIT 1 OFFSET n-1;

```

## Q-49. Write an SQL query to fetch departments along with the total salaries paid for each of them.
```
SELECT department, SUM(salary) FROM worker
GROUP BY department
;

```

## Q-50. Write an SQL query to fetch the names of workers who earn the highest salary.
```
SELECT first_name, salary FROM worker
WHERE salary = ( SELECT MAX(salary) FROM WORKER)
ORDER BY Salary DESC
;
```










[Source](https://www.techbeamers.com/sql-query-questions-answers-for-practice/)
