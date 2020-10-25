Suppose that a website contains two tables, the `Customers` table and the `Orders` table. Write a SQL query to find all customers who never order anything.

Table: `Customers`.

```
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```

Table: `Orders`.

```
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

Using the above tables as example, return the following:

```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

## 子查询

先从`Orders`里面找出所有购买过东西的顾客id组成一个集合A, 然后再判断customer里的每个顾客是否在A中.

```sql
# Write your MySQL query statement below
SELECT Name AS Customers FROM Customers WHERE Id NOT IN (
SELECT DISTINCT CustomerId FROM Orders
);
```

