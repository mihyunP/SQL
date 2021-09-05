### SQL 고득점 Kit

# SELECT

[모든 레코드 조회하기](https://programmers.co.kr/learn/courses/30/lessons/59034)

```sql
SELECT *
FROM ANIMAL_INS;
```

[역순 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/59035)

```sql
SELECT NAME, DATETIME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC;
```

[아픈 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59036)

```sql
-- 아픈 동물(INTAKE_CONDITION이 Sick 인 경우를 뜻함)
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION = 'Sick'
ORDER BY ANIMAL_ID;
```

[어린 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59037)

```sql
-- 젊은 동물(INTAKE_CONDITION이 Aged가 아닌 경우를 뜻함)
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION != 'AGED'
# WHERE INTAKE_CONDITION NOT IN('Aged')
ORDER BY ANIMAL_ID;
```

[동물의 아이디와 이름](https://programmers.co.kr/learn/courses/30/lessons/59403)

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

[여러 기준으로 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/59404)

```sql
SELECT ANIMAL_ID, NAME, DATETIME
FROM ANIMAL_INS
ORDER BY NAME ASC, DATETIME DESC;
```

[상위 n개 레코드](https://programmers.co.kr/learn/courses/30/lessons/59405)

```sql
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1;
-- 테이블의 레코드를 조회할 때 결과 중 상위 1 개 만 표시하려면)
-- MSSQL : SELECT TOP 1 가져 올 컬럼명
-- MYSQL : 맨 끝에 LIMIT 1
```



# SUM, MAX, MIN

[최댓값 구하기](https://programmers.co.kr/learn/courses/30/lessons/59415)

```SQL
SELECT DATETIME AS 시간
FROM ANIMAL_INS
ORDER BY DATETIME DESC 
LIMIT 1;
-- 별명 지정하기 : 컬럼명 AS 별명, 테이블명 AS 별명
```

[최솟값 구하기](https://programmers.co.kr/learn/courses/30/lessons/59038)

```SQL
# SELECT DATETIME AS 시간
# FROM ANIMAL_INS
# ORDER BY DATETIME
# LIMIT 1;

SELECT MIN(DATETIME) AS 시간
FROM ANIMAL_INS;
```

[동물 수 구하기](https://programmers.co.kr/learn/courses/30/lessons/59406)

```sql
SELECT COUNT(ANIMAL_ID)
FROM ANIMAL_INS;
```

[중복 제거하기](https://programmers.co.kr/learn/courses/30/lessons/59408)

```sql
SELECT COUNT(DISTINCT NAME) AS count
FROM ANIMAL_INS
WHERE NAME IS NOT NULL;

-- DISTINCT는 컬럼명 바로 앞에 붙인다.
```



# GROUP BY

[고양이와 개는 몇 마리 있을까](https://programmers.co.kr/learn/courses/30/lessons/59040)

```sql
# SELECT ANIMAL_TYPE, COUNT(ANIMAL_ID) AS count
# FROM ANIMAL_INS
# WHERE ANIMAL_TYPE = 'Cat'
# UNION
# SELECT ANIMAL_TYPE, COUNT(ANIMAL_ID) AS count
# FROM ANIMAL_INS
# WHERE ANIMAL_TYPE = 'Dog';

SELECT ANIMAL_TYPE, COUNT(ANIMAL_ID) AS count
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE;
```

[동명 동물 수 찾기](https://programmers.co.kr/learn/courses/30/lessons/59041)

```sql
SELECT NAME, COUNT(NAME) AS COUNT
FROM ANIMAL_INS
GROUP BY NAME
HAVING COUNT(NAME) >=2 AND NAME IS NOT NULL
ORDER BY NAME;
```

[입양 시각 구하기(1)](https://programmers.co.kr/learn/courses/30/lessons/59412)

```SQL
SELECT HOUR(DATETIME) AS HOUR, COUNT(ANIMAL_ID) AS COUNT
FROM ANIMAL_OUTS
GROUP BY HOUR(DATETIME)
HAVING HOUR BETWEEN 9 AND 19 
ORDER BY HOUR(DATETIME);

-- HAVING HOUR(DATETIME) BETWEEN 9 AND 19 
-- Unknown column 'DATETIME' in 'having clause'
-- SQL에서는 기본적으로 GROUP BY 절에 있거나 집계 함수가 사용된 컬럼만을 HAVING 절에서 사용할 수 있고, MySQL에서는 SELECT 문에 사용된 컬럼을 사용하는 것도 허용된다. 그런데 HOUR(DATETIME)는 컬럼 이름 앞에 함수가 붙었고, 그 함수가 집계 함수가 아니라 산술 함수이므로, HAVING절에서는 사용할 수 없는 것이다.
```

[입양 시각 구하기(2)](https://programmers.co.kr/learn/courses/30/lessons/59413)

```sql
#(sol 1)
SET @hour := -1; -- 변수 선언
SELECT (@hour := @hour + 1) as HOUR, (SELECT COUNT(*) 
                                      FROM ANIMAL_OUTS 
                                      WHERE HOUR(DATETIME) = @hour) as COUNT
FROM ANIMAL_OUTS
WHERE @hour < 23

# (sol 2)
# SELECT '0' AS HOUR, 
#     COUNT(IF(hour(datetime)=0, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '1' AS HOUR, 
#     COUNT(IF(hour(datetime)=1, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '2' AS HOUR, 
#     COUNT(IF(hour(datetime)=2, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '3' AS HOUR, 
#     COUNT(IF(hour(datetime)=3, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '4' AS HOUR, 
#     COUNT(IF(hour(datetime)=4, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '5' AS HOUR, 
#     COUNT(IF(hour(datetime)=5, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '6' AS HOUR, 
#     COUNT(IF(hour(datetime)=6, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '7' AS HOUR, 
#     COUNT(IF(hour(datetime)=7, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '8' AS HOUR, 
#     COUNT(IF(hour(datetime)=8, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '9' AS HOUR, 
#     COUNT(IF(hour(datetime)=9, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '10' AS HOUR, 
#     COUNT(IF(hour(datetime)=10, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '11' AS HOUR, 
#     COUNT(IF(hour(datetime)=11, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '12' AS HOUR, 
#     COUNT(IF(hour(datetime)=12, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '13' AS HOUR, 
#     COUNT(IF(hour(datetime)=13, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '14' AS HOUR, 
#     COUNT(IF(hour(datetime)=14, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '15' AS HOUR, 
#     COUNT(IF(hour(datetime)=15, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '16' AS HOUR, 
#     COUNT(IF(hour(datetime)=16, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '17' AS HOUR, 
#     COUNT(IF(hour(datetime)=17, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '18' AS HOUR, 
#     COUNT(IF(hour(datetime)=18, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '19' AS HOUR, 
#     COUNT(IF(hour(datetime)=19, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '20' AS HOUR, 
#     COUNT(IF(hour(datetime)=20, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '21' AS HOUR, 
#     COUNT(IF(hour(datetime)=21, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '22' AS HOUR, 
#     COUNT(IF(hour(datetime)=22, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
#     UNION ALL
# SELECT '23' AS HOUR, 
#     COUNT(IF(hour(datetime)=23, hour(datetime), NULL)) AS 'COUNT' 
#     FROM ANIMAL_OUTS
```



# IS NULL

[이름이 없는 동물의 아이디](https://programmers.co.kr/learn/courses/30/lessons/59039)

```sql
-- 코드를 입력하세요
SELECT ANIMAL_ID
FROM ANIMAL_INS 
WHERE NAME IS NULL
ORDER BY ANIMAL_ID;
```

[이름이 있는 동물의 아이디](https://programmers.co.kr/learn/courses/30/lessons/59407)

```SQL
SELECT ANIMAL_ID
FROM ANIMAL_INS 
WHERE NAME IS NOT NULL
ORDER BY ANIMAL_ID;
```

[NULL 처리하기](https://programmers.co.kr/learn/courses/30/lessons/59410)

```SQL
SELECT ANIMAL_TYPE, IF(NAME IS NULL,"No name",NAME) AS NAME, SEX_UPON_INTAKE
# SELECT ANIMAL_TYPE, IF(ISNULL(NAME),"No name",NAME) AS NAME, SEX_UPON_INTAKE
# SELECT ANIMAL_TYPE, IFNULL(NAME,"No name") AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;

-- 위의 3가지로 표현 가능함.
-- ISNULL, IFNULL 붙여써야함.
```



# JOIN

[없어진 기록 찾기](https://programmers.co.kr/learn/courses/30/lessons/59042)

```sql
-- (sol 1. 2LEFT OUTER JOIN)
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS AS OUTS
LEFT OUTER JOIN ANIMAL_INS AS INS
# ON OUTS.ANIMAL_ID = INS.ANIMAL_ID
USING (ANIMAL_ID)
WHERE INS.ANIMAL_ID IS NULL
ORDER BY OUTS.ANIMAL_ID;
-- 조인 조건 : 1. WHERE절, 2. ON절, 3. USING (컬럼명) 괄호 꼭쓰자

-- (sol 2. SUB QUERY)
# SELECT ANIMAL_ID, NAME
# FROM ANIMAL_OUTS
# WHERE ANIMAL_ID NOT IN (SELECT ANIMAL_ID
#                         FROM ANIMAL_INS)
# ORDER BY ANIMAL_ID;
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
-- 문제 잘 읽기! -> 중성화를 거치지 않은 동물은 성별 및 중성화 여부에 Intact, 중성화를 거친 동물은 Spayed 또는 Neutered라고 표시되어있습니다.

SELECT INS.ANIMAL_ID AS ANIMAL_ID, INS.ANIMAL_TYPE, INS.NAME
FROM ANIMAL_INS AS INS
JOIN ANIMAL_OUTS AS OUTS
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
# WHERE INS.SEX_UPON_INTAKE LIKE 'Intact%' 
# AND (OUTS.SEX_UPON_OUTCOME LIKE 'Neutered%' OR OUTS.SEX_UPON_OUTCOME LIKE 'Spayed%')
WHERE INS.SEX_UPON_INTAKE REGEXP 'Intact' 
AND (OUTS.SEX_UPON_OUTCOME REGEXP 'Neutered|Spayed')
ORDER BY ANIMAL_ID;
```



# String, Date

[루시와 엘라 찾기](https://programmers.co.kr/learn/courses/30/lessons/59046)

```sql
SELECT ANIMAL_ID,NAME,SEX_UPON_INTAKE
FROM ANIMAL_INS 
WHERE NAME REGEXP "^(Lucy|Ella|Pickle|Rogan|Sabrina|Mitty)$"
# WHERE NAME='Lucy' 
# OR NAME='Ella' 
# OR NAME='Pickle' 
# OR NAME='Rogan' 
# OR NAME='Sabrina' 
# OR NAME='Mitty';

-- REGEXP : LIKE를 이용한 검색과 달리 Regular Expression(정규 표현식)를 이용해 검색,   ^ㅣ작, 마$ㅣ막 
```

[이름에 el이 들어가는 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59047)

```SQL
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE NAME LIKE '%el%'
# WHERE (NAME LIKE '%el%'OR  NAME LIKE '%EL' OR NAME LIKE '%El' OR NAME LIKE '%eL')
AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME; -- 결과는 이름 순으로 조회

-- MY SQL 대소문자 구분안함 <- 컬럼명, 테이블명, SQL문은 대소문자 구분X, 문자열 데이터형은 대소문자 구분O?
-- WHERE절에서 대소문자 구별하려면? WHERE BINARY(NAME) LIKE '%el%' 소문자 el 포함한 데이터만 출력됨
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
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_INS AS INS
INNER JOIN ANIMAL_OUTS AS OUTS
ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
# ORDER BY DATEDIFF(OUTS.DATETIME,INS.DATETIME) DESC
--  DATEDIFF ( 종료날짜, 시작날짜 )
ORDER BY OUTS.DATETIME - INS.DATETIME DESC
LIMIT 2;
```

[DATETIME에서 DATE로 형 변환](https://programmers.co.kr/learn/courses/30/lessons/59414)

```sql
SELECT ANIMAL_ID, NAME,  DATE_FORMAT(DATETIME, '%Y-%m-%d') AS 날짜
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID;
```

