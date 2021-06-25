# SQL-Practice

Here is a little practice in SQL. 
The following questions and queries were created and answered using PostgreSQL through pdAdmin 4

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
Q-19. Write an SQL query to print details of the Workers whose SALARY lies between 100000 and 500000.
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

##


[Source](https://www.techbeamers.com/sql-query-questions-answers-for-practice/)
