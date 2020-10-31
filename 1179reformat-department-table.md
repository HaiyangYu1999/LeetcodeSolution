able: `Department`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| revenue       | int     |
| month         | varchar |
+---------------+---------+
(id, month) is the primary key of this table.
The table has information about the revenue of each department per month.
The month has values in ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"].
```

 

Write an SQL query to reformat the table such that there is a department id column and a revenue column **for each month**.

The query result format is in the following example:

```
Department table:
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+

Result table:
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+

Note that the result table has 13 columns (1 for the department id + 12 for the months).
```

## 1 暴力

每一个都嵌套一个子查询.

但是超时了. 好像leetcode sql的超时不用TLE而是Connection Timed Out. 第一次见到这种错的我还琢磨了老半天 😢

```sql
# Write your MySQL query statement below
select distinct t.id,
(select revenue from Department where month = 'Jan' and id = t.id) as Jan_Revenue,
(select revenue from Department where month = 'Feb' and id = t.id) as Feb_Revenue,
(select revenue from Department where month = 'Mar' and id = t.id) as Mar_Revenue,
(select revenue from Department where month = 'Apr' and id = t.id) as Apr_Revenue,
(select revenue from Department where month = 'May' and id = t.id) as May_Revenue,
(select revenue from Department where month = 'Jun' and id = t.id) as Jun_Revenue,
(select revenue from Department where month = 'Jul' and id = t.id) as Jul_Revenue,
(select revenue from Department where month = 'Aug' and id = t.id) as Aug_Revenue,
(select revenue from Department where month = 'Sep' and id = t.id) as Sep_Revenue,
(select revenue from Department where month = 'Oct' and id = t.id) as Oct_Revenue,
(select revenue from Department where month = 'Nov' and id = t.id) as Nov_Revenue,
(select revenue from Department where month = 'Dec' and id = t.id) as Dec_Revenue
from Department as t;
```

## 2 不使用子查询

可以使用聚合函数SUM和case语句

例如 通过case语句把表做个变换

```
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+
```

变为

```
id   Jan   Feb   Mar
1    8000  null  null
2    9000  null  null
3    null  10000 null
1    null  7000  null
1    null  null  6000
```

然后再对id分组求和即可

```sql
# Write your MySQL query statement below
select id,
sum(case month when 'Jan' then revenue else null end) as Jan_Revenue,
sum(case month when 'Feb' then revenue else null end) as Feb_Revenue,
sum(case month when 'Mar' then revenue else null end) as Mar_Revenue,
sum(case month when 'Apr' then revenue else null end) as Apr_Revenue,
sum(case month when 'May' then revenue else null end) as May_Revenue,
sum(case month when 'Jun' then revenue else null end) as Jun_Revenue,
sum(case month when 'Jul' then revenue else null end) as Jul_Revenue,
sum(case month when 'Aug' then revenue else null end) as Aug_Revenue,
sum(case month when 'Sep' then revenue else null end) as Sep_Revenue,
sum(case month when 'Oct' then revenue else null end) as Oct_Revenue,
sum(case month when 'Nov' then revenue else null end) as Nov_Revenue,
sum(case month when 'Dec' then revenue else null end) as Dec_Revenue
from department group by id order by id;
```

