# 🐱‍🐉코딩 테스트 SQL 중요 개념정리

MYSQL은 대소문자를 구분하지 않는다.

# ⚪SELECT

> ORDER BY

- ASC
- DESC

정렬 기준 2개일때

→ OOO ASC, OOO DESC

> LIMIT

- LIMIT [ 가져올 갯수 ]
  - LIMIT 1
- 특정 범위일 때
  - LIMIT [ 4, 10 ] → 5번째부터 10개 가져옴
  - 첫번째 인덱스는 파라미터 0부터 시작

# ⚪SUM, MAX, MIN, AVG, COUNT

- NULL인 경우는 집계하지 않는다.
- COUNT 의 경우
  - COUNT ( * ) → NULL도 포함된다.
  - COUNT ( 컬럼명 ) → NULL을 포함하지 않는다.

중복을 제거하는 경우 → COUNT ( DISTINCT OOO )

# ⚪GROUP BY

보통 집계함수와 같이 쓰이며 평균적으로 아래와 같은 형태로 쓰인다.

EX ) 고양이와 개가 각각 몇마리인지 조회하기

```mysql
SELECT ANIMAL_TYPE, COUNT( ANIMAL_TYPE )
GROUP BY ANIMAL_TYPE
```

[![image](https://user-images.githubusercontent.com/86418158/131340156-411e883c-93d5-4c4f-96e8-54a9fce3e6f0.png)](https://user-images.githubusercontent.com/86418158/131340156-411e883c-93d5-4c4f-96e8-54a9fce3e6f0.png)

> HAVING

- GROUP BY한 결과에 조건을 붙이고 싶을 때 사용

# ⚪IS NULL

> NULL인값을 구할 때

- = NULL (X)
- IS NULL(O)
- IS NOT NULL(O)

> IFNULL

- IFNULL( 값 1, 값 2 )
- 값 1이 NULL인 경우 값2로 표시한다.

# ⚪IF문

IF ( 조건문, 참일때의 값, 거짓일 때의 값 )

```mysql
IF( COLUM > 3 , "3보다크다", "3보다작다" )

A와 B가 들어갈 경우 O로 표시, 그렇지 않으면 X로 표시
IF((COLUM LIKE "%A%" OR COLUM LIKE "%B%"),'O','X') 
```

# ⚪JOIN

- A_TABLE JOIN B_TABLE

- USING : 두 테이블 간 필드 이름이 같을 경우에 사용, 괄호 꼭 쓰자!

  ```
  A_TABLE JOIN B_TABLE
  USING ( AB_COLUM ) 
  ```

- ON : 컬럼 이름이 다를 경우 사용, ( 같을 경우에 사용해도 됨 )

  ```
  A_TABLE A JOIN B_TABLE B
  ON A.AB_COLUM = B.AB_COLUM
  ```

> JOIN = INNER JOIN

[![image](https://user-images.githubusercontent.com/86418158/131340287-4c97a4ab-8ea7-40d1-9d73-d58431a536a4.png)](https://user-images.githubusercontent.com/86418158/131340287-4c97a4ab-8ea7-40d1-9d73-d58431a536a4.png)

RIGHT JOIN = RIGHT OUTER JOIN

[![image](https://user-images.githubusercontent.com/86418158/131340353-6ae02e93-23aa-4624-8091-bb1ebe61a0b5.png)](https://user-images.githubusercontent.com/86418158/131340353-6ae02e93-23aa-4624-8091-bb1ebe61a0b5.png)

LEFT JOIN = LEFT OUTER JOIN

[![image](https://user-images.githubusercontent.com/86418158/131340412-19b0310b-243c-43ca-b174-24a78da1218c.png)](https://user-images.githubusercontent.com/86418158/131340412-19b0310b-243c-43ca-b174-24a78da1218c.png)

- INNER JOIN 과 OUTER JOIN의 차이
  - `INNER` : TABLE A와 TABLE B의 교집합을 조회
  - `OUTER` : TABLE A와 TABLE B의 합집합을 조회
  - → OUTER JOIN보다 INNER JOIN을 사용할 경우 추가적으로 연산해야하는 타겟 데이터의 수가 확연히 줄기 때문에 쿼리 성능을 위해 INNER JOIN을 적절히 사용하는게 좋다.

> WHERE 조인과 JOIN 조인

- JOIN 을 사용하지 않는 조인
- WHERE , AND로 처리해버린다.

```mysql
FROM A , B
WHERE A.ANIMAL_ID = B.ANIMAL_ID AND B.DATETIME < A.DATETIM
```

- JOIN을 사용하는 조인
- JOIN조건은 ON에 작성한다.

```mysql
FROM A JOIN B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE O.DATETIME < I.DATETIME
```

# ⚪String, Date

- IN
  - 조건의 범위를 지정할 때 WHERE 절에 사용함
  - 서브쿼리와 주로 사용함

구할 것 : 특정 문자열이 포함된 문자열 구하기

- LIKE

  ```mysql
  WHERE (COLUM LIKE "%A%" OR COLUM LIKE "%B%") 
  ```

- REGEXP ( 특정 문자열이 여러개일 경우 )

  ```mysql
  WHERE COLUM REGEXP ('A|B')
  ```

DATE_FORMAT

> DATE의 형식을 바꿀 수 있다.

- Y : 4자리 연도
- y : 2자리 연도
- m : 월
- d : 일
- H : 24시간
- h : 12시간

```mysql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS 날짜
FROM ANIMAL_INS
```

[![image](https://user-images.githubusercontent.com/86418158/131340469-5acf1db1-7ee3-4417-83ea-bf2f1478456b.png)](https://user-images.githubusercontent.com/86418158/131340469-5acf1db1-7ee3-4417-83ea-bf2f1478456b.png)

# ⚪비슷한 함수들

> SUBSTRING

문자열의 일부를 잘라내기로 추출

SUBSTRING ( 문자열, 시작위치, 길이 )

EX) SUBSTRING ( ABC, 2, 1)→ B

※ 인덱스 시작은 1부터

> LEFT

문자열을 왼쪽부터 잘라내기

LEFT ( 문자열, 길이 ) 			EX) LEFT ( ABC, 2 ) → AB

> RIGHT

문자열을 오른쪽부터 잘라내기

RIGHT ( 문자열, 길이 ) 			EX) RIGHT ( ABC, 2 ) → BC

# ⚪SUB QUERY

구할 것 : B테이블에 없고, A테이블에만 있는 데이터 구하기

- 서브쿼리

```mysql
SELECT NAME, DATETIME
FROM A
WHERE ANIMAL_ID NOT IN (
    SELECT ANIMAL_ID
    FROM B
)
```

- LEFT JOIN

```mysql
SELECT A.NAME, A.DATETIME
FROM A LEFT JOIN B
USING(ANIMAL_ID)
WHERE B.ANIMAL_ID IS NULL
```

# ⚪ETC

> BETWEEN A AND B

- A 이상 B 이하

> ROUND

- 반올림
- ROUND ( 숫자, 보여질 자릿수 )
- 💥소수점 기준으로 1번째, -1번째 라고 생각하기
- EX ) ROUND ( 1.123 , 1 ) → 1.1
- EX ) ROUND ( 1.123 , -1 ) → 1

> TRUNCATE

- 버림
- TRUNCATE ( 숫자, 보여질 자릿수 )
- EX ) TRUNCATE ( 1.123 , 1 ) → 1.1





출처 : [chaeyeonkim0223](https://github.com/chaeyeonkim0223)