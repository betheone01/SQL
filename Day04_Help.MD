# Agenda

    - Practice Example
    - Joins

- Practice Examples

```SQL
-- display empid, end_date, on which he ended his previous job
SELECT EMPLOYEE_ID,MIN(END_DATE) FROM job_history GROUP BY
EMPLOYEE_ID ORDER BY EMPLOYEE_ID;
```

# Joins

- Initial Setup

```SQL
USE classwordk_db;

SOURCE <path of the joins.sql file>

SHOW TABLES;

SELECT * FROM emps;
SELECT * FROM depts;
SELECT * FROM addr;
SELECT * FROM meeting;
SELECT * FROM emp_meeting;
```

- Cross join

```SQL
--display ename and dname of the employee
SELECT ename,dname FROM emps CROSS JOIN depts;

SELECT e.ename,d.dname FROM emps e CROSS JOIN depts d;

SELECT emps.ename,depts.dname FROM emps CROSS JOIN depts;

SELECT e.ename,d.dname,d.deptno FROM emps e CROSS JOIN depts d;

SELECT e.ename,d.dname,d.deptno FROM emps AS e CROSS JOIN depts AS d;
```

- INNER JOIN

```SQL
--display empname and dept name
SELECT e.ename,d.dname FROM emps e
INNER JOIN depts d ON e.deptno = d.deptno;
-- equi-join

SELECT e.ename,d.dname FROM emps e
INNER JOIN depts d ON e.deptno > d.deptno;
--non-equi join
```

- LEFT OUTER JOIN

```SQL
--display empname,dname also show emps who are not assigned any depts
SELECT e.ename,d.dname FROM emps e
LEFT OUTER JOIN
depts d ON e.deptno = d.deptno;

SELECT e.ename,d.dname FROM emps e
LEFT JOIN
depts d ON e.deptno = d.deptno;
```

- RIGHT OUTER JOIN

```SQL
--display empname,dname also show dept names which are not assigned to any employees

SELECT e.ename,d.dname FROM emps e
RIGHT OUTER JOIN
depts d ON e.deptno=d.deptno;

SELECT e.ename,d.dname FROM emps e
RIGHT JOIN
depts d ON e.deptno=d.deptno;

SELECT e.ename,d.dname FROM depts d
RIGHT JOIN
emps e ON e.deptno=d.deptno;
```

- FULL OUTER JOIN

```SQL
SELECT e.ename,d.dname FROM emps e
FULL OUTER JOIN
depts d ON e.deptno=d.deptno;
-- not supported

-- FULL OUTER JOIN can be implemented with the help of Set Operators

-- UNION ALL will just combine the output of both the queries
SELECT e.ename,d.dname FROM emps e
LEFT JOIN
depts d ON e.deptno = d.deptno
UNION ALL
SELECT e.ename,d.dname FROM emps e
RIGHT JOIN
depts d ON e.deptno = d.deptno;

--UNION will combine the output of two queries and will consider the repeated data only once
SELECT e.ename,d.dname FROM emps e
LEFT JOIN
depts d ON e.deptno = d.deptno
UNION
SELECT e.ename,d.dname FROM emps e
RIGHT JOIN
depts d ON e.deptno = d.deptno;

```

- SELF JOIN

```SQL
-- display the emp name and the manager name
SELECT e.ename AS emp_name,m.ename AS mgr_name FROM emps e
INNER JOIN
emps m ON e.mgr = m.empno;

SELECT e.ename AS emp_name,m.ename AS mgr_name FROM emps e
LEFT JOIN
emps m ON e.mgr = m.empno;
```

# Joins Practice

```SQL
-- display empname and dname of all the employees
SELECT e.ename,d.dname FROM emps e
INNER JOIN depts d ON e.deptno = d.deptno;

-- display the emp name and district of that employee
SELECT e.ename, a.dist FROM emps e
INNER JOIN addr a ON e.empno = a.empno;

-- display empname,deptname and dist of the employee;
SELECT e.ename,d.dname,a.dist FROM emps e
INNER JOIN depts d ON e.deptno = d.deptno
INNER JOIN addr a ON e.empno=a.empno;

SELECT e.ename,d.dname,a.dist FROM emps e
LEFT JOIN depts d ON e.deptno = d.deptno
INNER JOIN addr a ON e.empno=a.empno;

-- display empname and the meeting name he is attending
SELECT e.ename,em.meetno FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno;

SELECT m.topic,em.empno FROM meeting m
INNER JOIN emp_meeting em ON m.meetno=em.meetno;

SELECT e.ename,m.topic FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno
INNER JOIN meeting m ON em.meetno = m.meetno;

-- Emps are travelling from  their hometown for meeting
-- display the empname,meeting topic and his home address
SELECT e.ename,a.tal, a.dist FROM emps e
INNER JOIN addr a ON e.empno = a.empno;

SELECT e.ename,m.topic FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno
INNER JOIN meeting m ON em.meetno = m.meetno;

SELECT e.ename,m.topic,a.tal,a.dist FROM emps e
INNER JOIN emp_meeting em ON e.empno=em.empno
INNER JOIN meeting m ON em.meetno=m.meetno
INNER JOIN addr a ON e.empno=a.empno;

--Emps are representing their depts in the meeting
-- display ename,deptname,meeting topic and venue of the meeting
SELECT e.empno,e.ename,d.dname,e.deptno FROM emps e
INNER JOIN depts d ON e.deptno = d.deptno;

SELECT e.empno,e.ename,m.topic,m.venue,e.deptno FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno
INNER JOIN meeting m ON em.meetno = m.meetno;

SELECT e.ename,m.topic,m.venue,d.dname FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno
INNER JOIN meeting m ON em.meetno = m.meetno
INNER JOIN depts d ON e.deptno = d.deptno;

--final
SELECT e.ename,m.topic,m.venue,d.dname FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno
INNER JOIN meeting m ON em.meetno = m.meetno;
LEFT JOIN depts d ON e.deptno = d.deptno

--display ename,dname,dist,venue of meeting
SELECT e.ename,d.dname,a.dist,m.venue FROM emps e
INNER JOIN addr a ON e.empno=a.empno
LEFT JOIN depts d ON d.deptno = e.deptno
INNER JOIN emp_meeting em ON em.empno=e.empno
INNER JOIN meeting m ON m.meetno=em.meetno;

--display dname, and count of employees
SELECT deptno,COUNT(empno) FROM emps GROUP BY deptno;

SELECT d.dname,COUNT(e.empno) FROM emps e
INNER JOIN depts d ON e.deptno = d.deptno
GROUP BY d.dname;

SELECT d.dname,COUNT(e.empno) FROM emps e
RIGHT JOIN depts d ON e.deptno = d.deptno
GROUP BY d.dname;


--display empname and its count of meetings attended
SELECT empno,COUNT(meetno) FROM emp_meeting GROUP BY empno ORDER BY empno;

SELECT e.ename,COUNT(em.meetno) FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno
GROUP BY e.ename;

SELECT e.ename,COUNT(em.meetno) FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno
GROUP BY e.ename
ORDER BY e.ename;

SELECT e.ename,COUNT(em.meetno) FROM emps e
INNER JOIN emp_meeting em ON e.empno = em.empno
GROUP BY e.ename
ORDER BY 2;

-- display all emps from 'dev' dept
SELECT e.ename FROM emps e
INNER JOIN depts d ON e.deptno = d.deptno
WHERE d.dname='dev';
```

# Usage of clauses as per priority

```SQL
SELECT columns FROM table1
XXX JOIN table2 ON condition
XXX JOIN table3 ON condition ...
WHERE condition
GROUP BY clause
HAVING condition
ORDER BY clause
LIMIT;
```

# Tomorrow's Agenda

    - Non Standard Joins
    - Create User
    - Grant Previleges
    - Table Locking
    - TCL
