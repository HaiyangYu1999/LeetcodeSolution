Write a SQL query to find all duplicate emails in a table named `Person`.

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

For example, your query should return the following for the above table:

```
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

**Note**: All emails are in lowercase.

## INNER JOIN

先根据Email自联结`select t1.Email from Person as t1, Person as t2 where t1.Email = t2.Email; `

| Id1  | Email1  | Id2  | Email2  |
| ---- | ------- | :--- | ------- |
| 1    | a@b.com | 1    | a@b.com |
| 1    | a@b.com | 3    | a@b.com |
| 2    | c@d.com | 2    | c@d.com |
| 3    | a@b.com | 1    | a@b.com |
| 3    | a@b.com | 3    | a@b.com |

再加上条件判断两个id是不是相等. 去掉相等的id, 就只剩下重复的了

| Id1  | Email1  | Id2  | Email2  |
| ---- | ------- | ---- | ------- |
| 1    | a@b.com | 3    | a@b.com |
| 3    | a@b.com | 1    | a@b.com |
最后再加一个distinct即可

```sql
# Write your MySQL query statement below
select distinct t1.Email from Person as t1, Person as t2 
where t1.Email = t2.Email and t1.Id != t2.Id; 
```

