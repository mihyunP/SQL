# ๐ฑโ๐์ฝ๋ฉ ํ์คํธ SQL ์ค์ ๊ฐ๋์ ๋ฆฌ

MYSQL์ ๋์๋ฌธ์๋ฅผ ๊ตฌ๋ถํ์ง ์๋๋ค.

# โชSELECT

> ORDER BY

- ASC
- DESC

์ ๋ ฌ ๊ธฐ์ค 2๊ฐ์ผ๋

โ OOO ASC, OOO DESC

> LIMIT

- LIMIT [ ๊ฐ์ ธ์ฌ ๊ฐฏ์ ]
  - LIMIT 1
- ํน์  ๋ฒ์์ผ ๋
  - LIMIT [ 4, 10 ] โ 5๋ฒ์งธ๋ถํฐ 10๊ฐ ๊ฐ์ ธ์ด
  - ์ฒซ๋ฒ์งธ ์ธ๋ฑ์ค๋ ํ๋ผ๋ฏธํฐ 0๋ถํฐ ์์

# โชSUM, MAX, MIN, AVG, COUNT

- NULL์ธ ๊ฒฝ์ฐ๋ ์ง๊ณํ์ง ์๋๋ค.
- COUNT ์ ๊ฒฝ์ฐ
  - COUNT ( * ) โ NULL๋ ํฌํจ๋๋ค.
  - COUNT ( ์ปฌ๋ผ๋ช ) โ NULL์ ํฌํจํ์ง ์๋๋ค.

์ค๋ณต์ ์ ๊ฑฐํ๋ ๊ฒฝ์ฐ โ COUNT ( DISTINCT OOO )

# โชGROUP BY

๋ณดํต ์ง๊ณํจ์์ ๊ฐ์ด ์ฐ์ด๋ฉฐ ํ๊ท ์ ์ผ๋ก ์๋์ ๊ฐ์ ํํ๋ก ์ฐ์ธ๋ค.

EX ) ๊ณ ์์ด์ ๊ฐ๊ฐ ๊ฐ๊ฐ ๋ช๋ง๋ฆฌ์ธ์ง ์กฐํํ๊ธฐ

```mysql
SELECT ANIMAL_TYPE, COUNT( ANIMAL_TYPE )
GROUP BY ANIMAL_TYPE
```

[![image](https://user-images.githubusercontent.com/86418158/131340156-411e883c-93d5-4c4f-96e8-54a9fce3e6f0.png)](https://user-images.githubusercontent.com/86418158/131340156-411e883c-93d5-4c4f-96e8-54a9fce3e6f0.png)

> HAVING

- GROUP BYํ ๊ฒฐ๊ณผ์ ์กฐ๊ฑด์ ๋ถ์ด๊ณ  ์ถ์ ๋ ์ฌ์ฉ

# โชIS NULL

> NULL์ธ๊ฐ์ ๊ตฌํ  ๋

- = NULL (X)
- IS NULL(O)
- IS NOT NULL(O)

> IFNULL

- IFNULL( ๊ฐ 1, ๊ฐ 2 )
- ๊ฐ 1์ด NULL์ธ ๊ฒฝ์ฐ ๊ฐ2๋ก ํ์ํ๋ค.

# โชIF๋ฌธ

IF ( ์กฐ๊ฑด๋ฌธ, ์ฐธ์ผ๋์ ๊ฐ, ๊ฑฐ์ง์ผ ๋์ ๊ฐ )

```mysql
IF( COLUM > 3 , "3๋ณด๋คํฌ๋ค", "3๋ณด๋ค์๋ค" )

A์ B๊ฐ ๋ค์ด๊ฐ ๊ฒฝ์ฐ O๋ก ํ์, ๊ทธ๋ ์ง ์์ผ๋ฉด X๋ก ํ์
IF((COLUM LIKE "%A%" OR COLUM LIKE "%B%"),'O','X') 
```

# โชJOIN

- A_TABLE JOIN B_TABLE

- USING : ๋ ํ์ด๋ธ ๊ฐ ํ๋ ์ด๋ฆ์ด ๊ฐ์ ๊ฒฝ์ฐ์ ์ฌ์ฉ, ๊ดํธ ๊ผญ ์ฐ์!

  ```
  A_TABLE JOIN B_TABLE
  USING ( AB_COLUM ) 
  ```

- ON : ์ปฌ๋ผ ์ด๋ฆ์ด ๋ค๋ฅผ ๊ฒฝ์ฐ ์ฌ์ฉ, ( ๊ฐ์ ๊ฒฝ์ฐ์ ์ฌ์ฉํด๋ ๋จ )

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

- INNER JOIN ๊ณผ OUTER JOIN์ ์ฐจ์ด
  - `INNER` : TABLE A์ TABLE B์ ๊ต์งํฉ์ ์กฐํ
  - `OUTER` : TABLE A์ TABLE B์ ํฉ์งํฉ์ ์กฐํ
  - โ OUTER JOIN๋ณด๋ค INNER JOIN์ ์ฌ์ฉํ  ๊ฒฝ์ฐ ์ถ๊ฐ์ ์ผ๋ก ์ฐ์ฐํด์ผํ๋ ํ๊ฒ ๋ฐ์ดํฐ์ ์๊ฐ ํ์ฐํ ์ค๊ธฐ ๋๋ฌธ์ ์ฟผ๋ฆฌ ์ฑ๋ฅ์ ์ํด INNER JOIN์ ์ ์ ํ ์ฌ์ฉํ๋๊ฒ ์ข๋ค.

> WHERE ์กฐ์ธ๊ณผ JOIN ์กฐ์ธ

- JOIN ์ ์ฌ์ฉํ์ง ์๋ ์กฐ์ธ
- WHERE , AND๋ก ์ฒ๋ฆฌํด๋ฒ๋ฆฐ๋ค.

```mysql
FROM A , B
WHERE A.ANIMAL_ID = B.ANIMAL_ID AND B.DATETIME < A.DATETIM
```

- JOIN์ ์ฌ์ฉํ๋ ์กฐ์ธ
- JOIN์กฐ๊ฑด์ ON์ ์์ฑํ๋ค.

```mysql
FROM A JOIN B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE O.DATETIME < I.DATETIME
```

# โชString, Date

- IN
  - ์กฐ๊ฑด์ ๋ฒ์๋ฅผ ์ง์ ํ  ๋ WHERE ์ ์ ์ฌ์ฉํจ
  - ์๋ธ์ฟผ๋ฆฌ์ ์ฃผ๋ก ์ฌ์ฉํจ

๊ตฌํ  ๊ฒ : ํน์  ๋ฌธ์์ด์ด ํฌํจ๋ ๋ฌธ์์ด ๊ตฌํ๊ธฐ

- LIKE

  ```mysql
  WHERE (COLUM LIKE "%A%" OR COLUM LIKE "%B%") 
  ```

- REGEXP ( ํน์  ๋ฌธ์์ด์ด ์ฌ๋ฌ๊ฐ์ผ ๊ฒฝ์ฐ )

  ```mysql
  WHERE COLUM REGEXP ('A|B')
  ```

DATE_FORMAT

> DATE์ ํ์์ ๋ฐ๊ฟ ์ ์๋ค.

- Y : 4์๋ฆฌ ์ฐ๋
- y : 2์๋ฆฌ ์ฐ๋
- m : ์
- d : ์ผ
- H : 24์๊ฐ
- h : 12์๊ฐ

```mysql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS ๋ ์ง
FROM ANIMAL_INS
```

[![image](https://user-images.githubusercontent.com/86418158/131340469-5acf1db1-7ee3-4417-83ea-bf2f1478456b.png)](https://user-images.githubusercontent.com/86418158/131340469-5acf1db1-7ee3-4417-83ea-bf2f1478456b.png)

# โช๋น์ทํ ํจ์๋ค

> SUBSTRING

๋ฌธ์์ด์ ์ผ๋ถ๋ฅผ ์๋ผ๋ด๊ธฐ๋ก ์ถ์ถ

SUBSTRING ( ๋ฌธ์์ด, ์์์์น, ๊ธธ์ด )

EX) SUBSTRING ( ABC, 2, 1)โ B

โป ์ธ๋ฑ์ค ์์์ 1๋ถํฐ

> LEFT

๋ฌธ์์ด์ ์ผ์ชฝ๋ถํฐ ์๋ผ๋ด๊ธฐ

LEFT ( ๋ฌธ์์ด, ๊ธธ์ด ) 			EX) LEFT ( ABC, 2 ) โ AB

> RIGHT

๋ฌธ์์ด์ ์ค๋ฅธ์ชฝ๋ถํฐ ์๋ผ๋ด๊ธฐ

RIGHT ( ๋ฌธ์์ด, ๊ธธ์ด ) 			EX) RIGHT ( ABC, 2 ) โ BC

# โชSUB QUERY

๊ตฌํ  ๊ฒ : Bํ์ด๋ธ์ ์๊ณ , Aํ์ด๋ธ์๋ง ์๋ ๋ฐ์ดํฐ ๊ตฌํ๊ธฐ

- ์๋ธ์ฟผ๋ฆฌ

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

# โชETC

> BETWEEN A AND B

- A ์ด์ B ์ดํ

> ROUND

- ๋ฐ์ฌ๋ฆผ
- ROUND ( ์ซ์, ๋ณด์ฌ์ง ์๋ฆฟ์ )
- ๐ฅ์์์  ๊ธฐ์ค์ผ๋ก 1๋ฒ์งธ, -1๋ฒ์งธ ๋ผ๊ณ  ์๊ฐํ๊ธฐ
- EX ) ROUND ( 1.123 , 1 ) โ 1.1
- EX ) ROUND ( 1.123 , -1 ) โ 1

> TRUNCATE

- ๋ฒ๋ฆผ
- TRUNCATE ( ์ซ์, ๋ณด์ฌ์ง ์๋ฆฟ์ )
- EX ) TRUNCATE ( 1.123 , 1 ) โ 1.1





์ถ์ฒ : [chaeyeonkim0223](https://github.com/chaeyeonkim0223)