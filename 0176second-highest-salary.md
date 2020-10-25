Write a SQL query to get the second highest salary from the `Employee` table.

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

For example, given the above Employee table, the query should return `200` as the second highest salary. If there is no second highest salary, then the query should return `null`.

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

## 1 LIMIT OFFSET

第一次尝试以失败告终, 原因是因为不知道怎么处理null的情况. 当表中只有1行时, SELECT只会返回empty set 而不是 NULL.

```sql
# error when occur the table contains only one row.
SELECT Salary AS SecondHighestSalary FROM Employee ORDER BY Salary DESC LIMIT 1 OFFSET 1;
```

## 2 ISNULL()函数

ISNULL(A, B) 当A为NULL的时候返回B. 这样就解决了返回NULL的情况了. 注意, 为了避免并列第一, 要用DISTINCT关键字

```sql
# Write your MySQL query statement below
SELECT IFNULL((SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT 1 OFFSET 1), NULL) AS SecondHighestSalary;
```

