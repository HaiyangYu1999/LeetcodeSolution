Mary is a teacher in a middle school and she has a table `seat` storing students' names and their corresponding seat ids.

The column **id** is continuous increment.

 

Mary wants to change seats for the adjacent students.

 

Can you write a SQL query to output the result for Mary?

 

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

For the sample input, the output is:

 

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```

**Note:**
If the number of students is odd, there is no need to change the last one's seat.

## case语句

```sql
# Write your MySQL query statement below
select t.id, (
select student from seat as t1 where t1.id = 
    (case when t.id % 2 = 0 then t.id - 1 
         when t.id % 2 = 1 and t.id < (select count(*) from seat) then t.id + 1
         else t.id end)
) as student from seat as t;
```

## 窗口函数

```sql
# Write your MySQL query statement below
select t.id, (case when id % 2 = 0 then lastStudent else nextStudent end) as student
from 
(select id, student,
        lag(student, 1, student) over(order by id) as lastStudent,
        lead(student, 1, student) over(order by id) as nextStudent
from seat) as t;
```

