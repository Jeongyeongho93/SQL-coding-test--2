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





Requirement meaning below
Among the animals given protection by center, write sql statements that look up the animals has been came into the center with given name.

-ORACLE
SELECT animal_id FROM animal_ins
WHERE name IS NOT NULL
ORDER BY animal_id ASC

-MYSQL
SELECT animal_id FROM animal_ins
WHERE name IS NOT NULL



Requirement meaning below
Post the animal info on adoption list. Write query to access animal species, name, sex and whether neutralizing in order to ID. You may consider that whom people does not know that the programming structure has null value, so that please mark up 'no name' which the animal has not been given name.

-ORACLE
1.
SELECT ANIMAL_TYPE, CASE WHEN NAME IS NULL THEN 'No name' ELSE NAME END NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC

2.
UPDATE ANIMAL_INS 
SET NAME = 'No name' 
WHERE NAME IS NULL
SELECT ANIMAL_TYPE, NAME, SEX_UPON_INTAKE 
FROM ANIMAL_INS
ORDER BY NAME ASC

3.
SELECT ANIMAL_TYPE, NVL(NAME, 'No name'), SEX_UPON_INTAKE FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC

-MYSQL
SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name') as NAME, SEX_UPON_INTAKE 
FROM ANIMAL_INS




Requirement meaning below
Due to unconsicous situation happened, so far some data has been missing currently. There was recorded an adoption process not only recorded come into animal shelter, please write query that access the animal id and name in order to id. Find the value that disappeared under other tables

-ORACLE
1.
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS
UNION ALL
SELECT ANIMAL_ID, NAME FROM ANIMAL_OUTS
ORDER BY ANIMAL_ID ASC

2.
SELECT ANIMAL_OUTS.ANIMAL_ID, ANIMAL_OUTS.NAME
FROM ANIMAL_OUTS LEFT OUTER JOIN ANIMAL_INS
ON ANIMAL_OUTS.ANIMAL_ID = ANIMAL_INS.ANIMAL_ID
WHERE ANIMAL_INS.ANIMAL_ID IS NULL
ORDER BY ANIMAL_OUTS.ANIMAL_I




















