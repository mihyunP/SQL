## 과목 1. 데이터 모델링의 이해

### 제1장. 데이터 모델링의 이해

- 데이터 모델링

: 개념적 > 논리적 > 물리적

개념적 => 추상화, ER모델(Entity-Relationship Model)

논리적 => 정확하게 표현

물리적 => 실제DB



* ER모델(Entity-Relationship Model)

:**개체**-**관계** 모델, **테이블** 간의 **관계**를 설명해주는 다이어그램



-  ERD(Entity-Relationship Diagram)

: 개체, 관계, 속성

: ERD 작성순서

1) 엔터티를 그린다.
2) 엔터티를 배치한다.
3) 엔터티 간의 관계를 설정한다.
4) 관계명을 기술한다.
5) 관계의 참여도를 기술한다.
6) 관계의 필수여부를 기술한다.



-> **피터첸 표기법**

![image-20230430155540252](C:\Users\mihyun\AppData\Roaming\Typora\typora-user-images\image-20230430155540252.png)

-> **IE 표기법**

![image-20230430171908701](C:\Users\mihyun\AppData\Roaming\Typora\typora-user-images\image-20230430171908701.png)

![image-20230430223359009](C:\Users\mihyun\AppData\Roaming\Typora\typora-user-images\image-20230430223359009.png)

![image-20230430233933984](C:\Users\mihyun\AppData\Roaming\Typora\typora-user-images\image-20230430233933984.png)

-> **Baker 표기법**

![image-20230430223642941](C:\Users\mihyun\AppData\Roaming\Typora\typora-user-images\image-20230430223642941.png)

![image-20230430233945551](C:\Users\mihyun\AppData\Roaming\Typora\typora-user-images\image-20230430233945551.png)



- 용어정리

개체(`앤터티` )- table, 릴레이션 cf)  객체(Object)

: 두개 이상의 인스턴스, 두개 이상의 속성 존재함, 다른 엔터티와 최소 1개 이상의 관계O

유형,개념,사건 / 기본,중심,행위

`인스턴스` - rows(행), 튜플, 가로



`속성` - columns(열), 세로

: 인스턴스에서 관리하고자 하는 의미상 더이상 분리되지 않는 최소 데이터 단위

 기본/설계/파생

`도메인` - 각 속성이 가질 수 있는 값의 범위(데이터타입, 크기, 제약사항 등 지정)



`관계`

: 개체 사이의 연관성을 나타내는 개념, 동사, 관계명, 관계차수(cardinality), 관계선택사양, UML

존재에 의한 관계/ 행위에 의한 관계

cf) UML : 개발자간의 의사소통을 원활하게 하기 위해 표준화한 모델링 언어,

​	연관관계(실선) - 항상 이용하는 관계 ex) 소속된다

​	의존관계(점선) - 상대 행위에 의해 발생하는 관계 ex) 주문한다



`식별자` - 엔티티 내에서 인스턴스를 구분하는 구분자

특징 : 유일성, 최소성, 불변성, 존재성

분류 : 주식별자, 보조식별자 / 내부식별자, 외부식별자 / 단일 식별자, 복합 식별자 / 본질 식별자, 인조 식별자



![image-20230430233746581](C:\Users\mihyun\AppData\Roaming\Typora\typora-user-images\image-20230430233746581.png)





-> Q. ERD를 그리는 방법 : 피터첸 / IE, Baker? 3가지

-> ERD 부모 엔터티, 자식 엔터티 구분하는 방법?

->UML이란? 관계 표현하는 수단?



### 제2장. 데이터 모델과 성능

성능 데이터 모델링

`정규화`: 반복적인 데이터를 분리하고 각 데이터가 종속된 테이블에 적절하게 배치되도록 하는 것

`반정규화` : 테이블 반정규화, 칼럼 반정규화, 관계 반정규화

`분산DB` : 여러 곳으로 분산되어있는 DB를 하나의 가상 시스템으로 사용할 수 있도록 한 DB, 논리적 동일&물리적 분산 데이터 집합



## 과목 2. SQL 기본 및 활용

### 제1장. SQL 기본

DB : 특정 기업이나 조직 또는 개인이 필요에 의해 데이터를 일정한 형태로 저장해 놓은 것

DBMS : 효율적인 데이터 관리 뿐만 아니라 예기치 못한 사건으로 인한 데이터의 손상을 피하고, 필요시 필요한 데이터를 복구하기 위한 강력한 기능의 SW

SQL : 관계형 DB에서 데이터의 정의, 조작, 제어를 위해 사용하는 언어

| DML     | 데이터 조회/변형<br />입력, 수정, 삭제, 조회<br />(DB 실제 접근하는데 사용) | SELECT, INSERT, UPDATE, DELETE          |
| ------- | ------------------------------------------------------------ | --------------------------------------- |
| **DDL** | **데이터 구조 정의<br />객체 생성/변경/제거<br />(스키마, 도메인, 테이블, 뷰, 인덱스 정의/변경/제거)<br />AUTOCOMMIT** | **CREATE, ALTER, DROP, RENAME, MODIFY** |
| **DCL** | **권한 부여/회수**                                           | **GRANT, REVOKE**                       |
| **TCL** | **작업 완료/취소**                                           | **COMMIT, ROLLBACK, SAVEPOINT**         |



`DROP` VS `TRUNCATE` VS `DELETE` 차이점

: 테이블 구조까지X VS 디스크 사용량 초기화 VS 테이블 데이터 모두 삭제



`DISTINCT` deptno, mgr == GROUP BY  deptno, mgr



`와일드 카드` : % , _, [charlist],[!charlist] cf) LIKE 연산자

`이스케이프` : @를 와일드 카드 앞에 붙이면 와일드 카드를 문자로 취급 cf) LIKE 'A_A' VS LIKE 'A@_A'



`AS` : 1) SELECT절 생략가능 2) FROM절 사용불가 

문자열 연결 : [ORACLE] `||`, [SQL Server] `+`

`SELECT 문장 실행 순서`: FROM - WHERE - GROUP BY - HAVING - SELECT - ORDER BY



`SAVEPOINT` : 
[ORACLE]

```oracle
SAVEPOINT SVPT1;

...
ROLLBACK TO SVPT1;
```

[SQL Server]

```mysql
SAVE TRANSACTION SVTR1;

...
ROLLBACK TO SVTR1;
```

논리 연산자 우선순위 : NOT > AND > OR 순



NULL의 연산

1) NULL의 사칙연산(+_*/) => NULL
2) NULL의 비교연산(=,>,>=,<,<=) => FALSE
3) 정렬상 의미 : [ORACLE] 맨 마지막에 위치, [SQL Server] 맨 처음에 위치
4) NULL값을 조건절에서 사용하는 경우 IS NULL, IS NOT NULL 키워드를 사용해야한다.

`''` 의미 : [ORACLE]에서는 NULL, [SQL Server]에서는 ''



단일행 함수 : 문자형 함수/ 숫자형 함수 / 날짜형 함수 / 변환형 함수/ NULL 함수
다중행 함수 : 그룹함수 / 집계함수 등 여러행을 바탕으로 하나의 결과값 도출 함수



JOIN : 두 개 이상의 테이블들을 연결 또는 결합하여 데이터를 출력하는 것
5가지 테이블을 JOIN하기 위해서는 최소 4번의 JOIN 과정이 필요하다. (N-1)



### **제2장. SQL 활용**

NOT EXIST == IS NULL

`집합 연산자` : 두 개 이사의 테이블에서 조인을 사용하지 않고 연관된 데이터를 조회할 때 사용, 테이블 간 컬럼 동일할 때만 사용 가능

ex) UNION, UNION ALL , INTERSECT , MINUS(EXCEPT), CROSS JOIN



`서브쿼리`: 하나의 SQL문 안에 포함되어 있는 또 다른 SQL문, 알려지지 않은 기준을 이용한 검색에 사용



`뷰`: 가상의 테이블, 실제 데이터를 가지고 있지 않음			cf) 테이블



### 제3장. SQL 최적화 기본 원리

