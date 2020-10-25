Table: `Person`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId is the primary key column for this table.
```

Table: `Address`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId is the primary key column for this table.
```

 

Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:

```
FirstName, LastName, City, State
```

## 左连接

如果使用INNER JOIN, 在测试样例中, 有的PersonId只在表Person中出现, 但是答案要求没在Address中出现的要用NULL. 所以只能用表Person LEFT OUTER JOIN Address.

```sql
# Write your MySQL query statement below
SELECT FirstName, LastName, City, State FROM Person LEFT OUTER JOIN Address ON Person.PersonId = Address.PersonId;
```

