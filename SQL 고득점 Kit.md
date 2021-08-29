### SQL 고득점 Kit

# SELECT

[모든 레코드 조회하기](https://programmers.co.kr/learn/courses/30/lessons/59034)

```sql
-- 코드를 입력하세요
SELECT * 
FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC;
```

[역순 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/59035)

```sql
-- 코드를 입력하세요
SELECT NAME, DATETIME 
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC;
```

[아픈 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59036)

```sql
-- 코드를 입력하세요
SELECT animal_id, name 
from animal_ins 
where intake_condition ='Sick' 
order by animal_id ;
```

[어린 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59037)

```sql
-- 코드를 입력하세요
SELECT animal_id, name 
from animal_ins 
where intake_condition not in('Aged')
order by animal_id;
```

[동물의 아이디와 이름](https://programmers.co.kr/learn/courses/30/lessons/59403)

```sql
-- 코드를 입력하세요
SELECT animal_id, name 
from animal_ins 
order by animal_id;
```

[여러 기준으로 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/59404)

```sql
-- 코드를 입력하세요
SELECT ANIMAL_ID, name, datetime 
from ANIMAL_INS 
order by name asc, datetime desc;
```

[상위 n개 레코드](https://programmers.co.kr/learn/courses/30/lessons/59405)

```sql
-- 코드를 입력하세요
SELECT name from animal_ins
order by datetime asc
limit 1;
```



# SUM, MAX, MIN

[최댓값 구하기](https://programmers.co.kr/learn/courses/30/lessons/59415)

```SQL
-- 코드를 입력하세요
SELECT datetime from animal_ins
order by datetime desc
limit 1;
```

[최솟값 구하기](https://programmers.co.kr/learn/courses/30/lessons/59038)

```SQL
-- 코드를 입력하세요
SELECT min(datetime) as '시간' 
from animal_ins;
```

[동물 수 구하기](https://programmers.co.kr/learn/courses/30/lessons/59406)

```sql
-- 코드를 입력하세요
SELECT count(animal_id) as 'count' 
from animal_ins;
```

[중복 제거하기](https://programmers.co.kr/learn/courses/30/lessons/59408)

```sql
-- 코드를 입력하세요
SELECT count(distinct name) as 'count' 
from animal_ins 
where name is not null;
```



# GROUP BY

[고양이와 개는 몇 마리 있을까](https://programmers.co.kr/learn/courses/30/lessons/59040)

```sql
-- 코드를 입력하세요
SELECT animal_type, count(animal_id) as 'count' from animal_ins 
group by animal_type
order by animal_type;
```

[동명 동물 수 찾기](https://programmers.co.kr/learn/courses/30/lessons/59041)

```sql
-- 코드를 입력하세요
SELECT NAME, COUNT(NAME) 
FROM ANIMAL_INS
GROUP BY NAME HAVING COUNT(NAME)>=2
ORDER BY NAME;
```

[입양 시각 구하기(1)](https://programmers.co.kr/learn/courses/30/lessons/59412)

```SQL
-- 코드를 입력하세요
SELECT hour(datetime) AS 'HOUR', count(animal_id) AS 'COUNT' 
from animal_outs
group by hour(datetime)
having HOUR BETWEEN 9 AND 19
order by hour(datetime);
```

[입양 시각 구하기(2)](https://programmers.co.kr/learn/courses/30/lessons/59413)

```sql
# -- 코드를 입력하세요

# SELECT HOUR(DATETIME) AS 'HOUR', COUNT(ANIMAL_ID) AS 'COUNT' FROM ANIMAL_OUTS

# GROUP BY HOUR HAVING HOUR BETWEEN 0 AND 23

# ORDER BY HOUR ASC;

SET @hour := -1; -- 변수 선언

SELECT (@hour := @hour + 1) as HOUR,
(SELECT COUNT(*) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @hour) as COUNT
FROM ANIMAL_OUTS
WHERE @hour < 23
```



# IS NULL

[이름이 없는 동물의 아이디](https://programmers.co.kr/learn/courses/30/lessons/59039)

```sql
-- 코드를 입력하세요
SELECT animal_id 
from animal_ins
where name is null
order by animal_id;
```

[이름이 있는 동물의 아이디](https://programmers.co.kr/learn/courses/30/lessons/59407)

```SQL
-- 코드를 입력하세요
SELECT animal_id 
from animal_ins
where name is not null
order by animal_id;
```

[NULL 처리하기](https://programmers.co.kr/learn/courses/30/lessons/59410)

```SQL
-- 코드를 입력하세요
SELECT animal_type, IFNULL(name,'No name') as 'NAME', sex_upon_intake 
from animal_ins
order by animal_id asc;
```



# JOIN

[없어진 기록 찾기](https://programmers.co.kr/learn/courses/30/lessons/59042)

```sql
-- 코드를 입력하세요
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_INS AS INS
RIGHT JOIN ANIMAL_OUTS AS OUTS
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.ANIMAL_ID IS NULL;
```

[있었는데요 없었습니다](https://programmers.co.kr/learn/courses/30/lessons/59043)

```sql
-- 코드를 입력하세요
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS AS OUTS 
LEFT OUTER JOIN ANIMAL_INS AS INS
ON OUTS.ANIMAL_ID = INS.ANIMAL_ID
WHERE OUTS.DATETIME < INS.DATETIME
ORDER BY INS.DATETIME

-- ANIMAL_ID가 ANIMAL_OUTS에는 있고 ANIMAL_INS에는 없는 동물의 ANIMAL_ID, NAME
```

[오랜 기간 보호한 동물(1)](https://programmers.co.kr/learn/courses/30/lessons/59044)

```sql
-- 코드를 입력하세요 TOP3
SELECT INS.NAME, INS.DATETIME
FROM ANIMAL_INS AS INS
LEFT OUTER JOIN ANIMAL_OUTS AS OUTS
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE OUTS.ANIMAL_ID IS NULL
ORDER BY INS.DATETIME
LIMIT 3;
```

[보호소에서 중성화한 동물](https://programmers.co.kr/learn/courses/30/lessons/59045)

```SQL
-- 코드를 입력하세요
SELECT INS.ANIMAL_ID AS ANIMAL_ID, INS.ANIMAL_TYPE, INS.NAME
FROM ANIMAL_INS AS INS
JOIN ANIMAL_OUTS AS OUTS
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.SEX_UPON_INTAKE LIKE 'Intact%' 
AND (OUTS.SEX_UPON_OUTCOME LIKE 'Neutered%' OR OUTS.SEX_UPON_OUTCOME LIKE 'Spayed%')

# UNION

# SELECT INS.ANIMAL_ID AS ANIMAL_ID, INS.ANIMAL_TYPE ,INS.NAME 

# FROM ANIMAL_INS AS INS

# JOIN ANIMAL_OUTS AS OUTS

# ON INS.ANIMAL_ID = OUTS.ANIMAL_ID

# WHERE INS.SEX_UPON_INTAKE LIKE 'Intact%'

# AND OUTS.SEX_UPON_OUTCOME LIKE 'Spayed%'

ORDER BY ANIMAL_ID;
-- 중성화를 거치지 않은 동물은 성별 및 중성화 여부에 Intact, 중성화를 거친 동물은 Spayed 또는 Neutered라고 표시되어있습니다.
```



# String, Date

[루시와 엘라 찾기](https://programmers.co.kr/learn/courses/30/lessons/59046)

```sql
-- 코드를 입력하세요
SELECT ANIMAL_ID,NAME,SEX_UPON_INTAKE
FROM ANIMAL_INS 
WHERE NAME REGEXP "^(Lucy|Ella|Pickle|Rogan|Sabrina|Mitty)$"

# WHERE NAME='Lucy' 

# OR NAME='Ella' 

# OR NAME='Pickle' 

# OR NAME='Rogan' 

# OR NAME='Sabrina' 

# OR NAME='Mitty';
```

[이름에 el이 들어가는 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59047)

```SQL
-- 코드를 입력하세요
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS 
WHERE (NAME LIKE '%el%'OR  NAME LIKE '%EL' OR NAME LIKE '%El' OR NAME LIKE '%eL')
AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME; -- 결과는 이름 순으로 조회
```

[중성화 여부 파악하기](https://programmers.co.kr/learn/courses/30/lessons/59409)

```sql
-- 코드를 입력하세요 : 동물의 아이디와 이름, 중성화 여부를 아이디 순으로 조회하는 SQL문
SELECT ANIMAL_ID, NAME, IF(SEX_UPON_INTAKE LIKE 'Neutered%' OR  SEX_UPON_INTAKE LIKE 'Spayed%',"O","X") AS 중성화
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

[오랜 기간 보호한 동물(2)](https://programmers.co.kr/learn/courses/30/lessons/59411)

```sql
-- 코드를 입력하세요
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_INS AS INS
INNER JOIN ANIMAL_OUTS AS OUTS
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
ORDER BY DATEDIFF(OUTS.DATETIME,INS.DATETIME) DESC
--  DATEDIFF ( 종료날짜, 시작날짜 )

# ORDER BY OUTS.DATETIME - INS.DATETIME DESC

LIMIT 2;
```

[DATETIME에서 DATE로 형 변환](https://programmers.co.kr/learn/courses/30/lessons/59414)

```sql
-- 코드를 입력하세요 : 각 동물의 아이디와 이름, 들어온 날짜1를 조회하는 SQL문을 작성해주세요. 이때 결과는 아이디 순으로 조회
SELECT ANIMAL_ID, NAME,  DATE_FORMAT(DATETIME, '%Y-%m-%d') AS 날짜
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID;
```

