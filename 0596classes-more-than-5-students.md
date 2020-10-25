There is a table `courses` with columns: **student** and **class**

Please list out all classes which have more than or equal to 5 students.

For example, the table:

```
+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+
```

Should output:

```
+---------+
| class   |
+---------+
| Math    |
+---------+
```

 

**Note:**
The students should not be counted duplicate in each course.

## GROUP BY的使用

按照class分组, 统计每个组的学生个数, 选出大于等于5的组来.

但是要注意, 在测试样例中, (A, Math)这样的元组可能出现多次, 不要统计相同的学生和相同的课. 要用COUNT(DISTINCT name)

```sql
# Write your MySQL query statement below
SELECT class FROM courses GROUP BY class HAVING count(DISTINCT student) >= 5;
```

