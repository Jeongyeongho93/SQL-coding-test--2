# SQL-coding-test--2

Requirement meaning below
The protection center wants to know what time the adoption most be active while 9 am and 19:59pm, Please make SQL query statement that How many cases of adoption process ongoing each of hours

-ORACLE
select hour, count(*)
select hour, count(*) from 
(select to_number(to_char(datetime,'HH24')) as hour
from animal_outs) 
where hour between 9 and 19 
group by hour
order by hour

-MYSQL
select hour(datetime) as hour, count(hour(datetime)) as count 
from animal_outs
group by hour
having hour between 9 and 19
order by hour




Requirement meaning below 
--- "For this question that not only suggest how to deal with write query statement 
but also check how the tester understand common table expression based on WITH RECURSIVE STATEMENT" ---
The protection center wants to know what time the adoption most be active while 0 am and 23:59pm

-ORACLE
SELECT B.HH, COUNT(A.HOUR)
FROM   (SELECT TO_CHAR(DATETIME, 'HH24') AS HOUR 
        FROM   ANIMAL_OUTS) A 
RIGHT OUTER JOIN 
      (SELECT ROWNUM-1 AS HH FROM DUAL CONNECT BY LEVEL <= 24) B
ON    A.HOUR = B.HH
GROUP BY B.HH
ORDER BY B.HH

-MYSQL
1.
set @time=-1;
select @time:=@time+1,
  (select count(datetime) 
  from ANIMAL_OUTS 
  where @time=hour(datetime))
from ANIMAL_OUTS
where @time<23
order by @time

2.
WITH RECURSIVE TIME AS (
    SELECT 0 HOUR
    UNION ALL
    SELECT HOUR + 1 FROM TIME WHERE HOUR < 23
)

SELECT HOUR, COUNT(*) - 1 COUNT
FROM (
    SELECT HOUR(DATETIME) HOUR FROM ANIMAL_OUTS
    UNION ALL
    SELECT HOUR FROM TIME
    ) VALID
GROUP BY HOUR
ORDER BY HOUR




Requirement meaning below
Among the animals given protection by center, write sql statements that look up the animals has been came into the center without any given name.

-ORACLE
SELECT animal_id FROM animal_ins
WHERE name IS NULL
ORDER BY animal_id ASC

-MYSQL
SELECT animal_id FROM animal_ins
WHERE name IS NULL








