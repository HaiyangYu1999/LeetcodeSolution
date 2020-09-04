Write a SQL query to **delete** all duplicate email entries in a table named `Person`, keeping only unique emails based on its *smallest* **Id**.

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id is the primary key column for this table.
```

For example, after running your query, the above `Person` table should have the following rows:

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```

**Note:**

Your output is the whole `Person` table after executing your sql. Use `delete` statement.

## 1 子查询1(报错)

先构造一个集合, 这个集合包含所有的重复邮箱的Id `SELECT p1.Id FROM Person AS p1, Person AS p2 WHERE p1.Email = p2.Email AND p1.Id > p2.Id`

然后再删除在这个集合中的邮箱.

```mysql
# error You can't specify target table 'p' for update in FROM clause
DELETE FROM Person WHERE Id IN 
(
	SELECT p1.Id FROM Person AS p1, Person AS p2 WHERE p1.Email = p2.Email AND p1.Id > p2.Id
);
```

**错误的原因是子查询和删除操作不能使用同一张表!**

## 2 子查询2

解决子查询1的做法是, 把子查询的结果重命名一下, 重命名到`Tmp`上, 这样就不会产生问题了

```mysql
# Write your MySQL query statement below
DELETE FROM Person WHERE Id IN 
(
	SELECT Id FROM (SELECT p1.Id FROM Person AS p1, Person AS p2 WHERE p1.Email = p2.Email AND p1.Id >
                    p2.Id) AS Tmp
);
```

