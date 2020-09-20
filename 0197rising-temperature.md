Table: `Weather`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id is the primary key for this table.
This table contains information about the temperature in a certain day.
```

 

Write an SQL query to find all dates' `id` with higher temperature compared to its previous dates (yesterday).

Return the result table in **any order**.

The query result format is in the following example:

```
Weather
+----+------------+-------------+
| id | recordDate | Temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+

Result table:
+----+
| id |
+----+
| 2  |
| 4  |
+----+
In 2015-01-02, temperature was higher than the previous day (10 -> 25).
In 2015-01-04, temperature was higher than the previous day (30 -> 20).
```

## JOIN

思路是连接两个表, 第n天的信息去和第n-1天的信息连接. 如果第n-1天的温度小于第n天的温度, 那么就返回这个值.否则不返回这个值

注意date类型的加减法是用`date_add()`实现的

```mysql
# Write your MySQL query statement below
SELECT t1.id FROM Weather AS t1, Weather AS t2 WHERE t1.recordDate = date_add(t2.recordDate, interval 1 day) AND t1.Temperature > t2.Temperature;
```

