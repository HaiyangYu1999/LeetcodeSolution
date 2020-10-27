The `Employee` table holds all employees. Every employee has an Id, a salary, and there is also a column for the department Id.

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```

The `Department` table holds all departments of the company.

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

Write a SQL query to find employees who have the highest salary in each of the departments. For the above tables, your SQL query should return the following rows (order of rows does not matter).

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

**Explanation:**

Max and Jim both have the highest salary in the IT department and Henry has the highest salary in the Sales department.

## 1 窗口函数

不得不说窗口函数真的很好用, 好用的让人上瘾. 尤其是分组求top n.

```sql
# Write your MySQL query statement below
select t2.Name as Department, t1.Name as Employee, 
t1.Salary from (select DepartmentId, Name, Salary, dense_rank() over(partition by DepartmentId order by Salary desc) as rnk from Employee) as t1 
inner join Department as t2 on t1.DepartmentId = t2.Id where t1.rnk = 1;
```

## 2 聚合函数 + group by

用max和group by也能解决问题. 但是换个题, 比如求每个部门前两名高的salary就无能为力了

**同时要注意两字段的IN**

```sql
# Write your MySQL query statement below

select t2.Name as Department, t1.Name as Employee, t1.Salary 
from Employee as t1 inner join Department as t2 on t1.DepartmentId = t2.Id 
where (t1.Salary, t1.DepartmentId) in (select max(Salary), DepartmentId from Employee group by DepartmentId);
```

