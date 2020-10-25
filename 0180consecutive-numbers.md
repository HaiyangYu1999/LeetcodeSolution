Write a SQL query to find all numbers that appear at least three times consecutively.

```
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```

For example, given the above `Logs` table, `1` is the only number that appears consecutively for at least three times.

```
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

## 内连接

连接2次, 找到连续出现3次的值之后去重

```sql
# Write your MySQL query statement below
select distinct t1.Num as ConsecutiveNums from logs as t1, logs as t2, logs as t3 where t1.num = t2.num and t2.num = t3.num and t1.Id + 1 = t2.Id and t2.Id + 1 = t3.Id;
```

