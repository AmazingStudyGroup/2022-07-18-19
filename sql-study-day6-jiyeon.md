### 05-3 연산자 종류와 활용 방법 알아보기

#### 산술연산자
```SQL
SELECT * 
   FROM EMP
 WHERE SAL * 12 = 36000;
-- WHERE절의 조건식에도 산술연산자를 사용할 수 있다.
```

#### 비교연산자
```sql
SELECT *
   FROM EMP
 WHERE SAL >= 3000;
```
> 급여가 2500 이상이고 직업이 ANALYST인 사원 정보만 나오게 코드짜보기!
```sql
SELECT * 
   FROM EMP
 WHERE SAL >= 2500 AND JOB = 'ANALYST';
```
> 대소비교 연산자는 비교 대상인 데이터가 숫자가 아닌 문자열일 때도 사용할 수 있음

- 등가비교 연산자 : = , != , <> , ^=
```sql
SELECT *
   FROM EMP
 WHERE SAL != 3000;
  
SELECT *
   FROM EMP
 WHERE SAL <> 3000;
  
SELECT * 
   FROM EMP
 WHERE SAL ^= 3000;
```
> 실무에서는 ^= 보다 != 와 <>를 더 많이 사용한다

#### 논리 부정 연산자(NOT)
```SQL
SELECT * 
   FROM EMP
 WHERE NOT SAL = 3000;
```
> 복잡한 여러 개 조건식이 AND, OR로 묶여 있는 상태에서 정반대의 결과를 얻고자 할 때 유용하게 사용가능

#### IN 연산자
```sql
-- OR 연산자를 사용해서 직업이 'MANAGER'이거나, 'SALESMAN'이거나, 'CLERK'인 데이터 출력하기
SELECT *
   FROM EMP
 WHERE JOB = 'MANAGER'
    OR JOB = 'SALESMAN'
    OR JOB = 'CLERK';

-- IN 연산자를 사용하여 특정 열에 해당하는 조건을 여러 개 지정할 수 있음
SELECT *
   FROM EMP
 WHERE JOB IN('MANAGER', 'SALESMAN', 'CLERK');
```

#### BETWEEN A AND B 연산자
```sql
-- 대소 비교 연산자와 AND 연산자를 함께 사용해서 범위를 표현할 수 있음
SELECT *
   FROM EMP
 WHERE SAL >= 2000
   AND SAL <= 3000;
   
-- BETWEEN A AND B 연산자를 사용하여 최소/최고 범위를 더 간단하게 표현 가능
SELECT *
   FORM EMP
 WHERE SAL BETWEEN 2000 AND 3000;
```

#### LIKE 연산자와 와일드 카드
```sql
-- 이름이 S로 시작하는 데이터 출력하기
SELECT * 
   FROM EMP
 WHERE ENAME LIKE 'S%';

-- 이름의 두번째 글자가 L인 사원 데이터만 출력하기
SELECT *
   FROM EMP
 WHERE ENAME LIKE '_L%';
 
-- 이름에 AM이 포함되어 있는 사원 데이터만 출력하기
SELECT *
   FROM EMP
 WHERE ENAME LIKE '%AM%';
```
> LIKE 연산자와 함께 사용할 수 있는 와일드카드는 _와 %이다.    
> _ : 어떤 값이든 상관없이 **한 개의 문자 데이터**를 의미      
> % : **길이와 상관없이**(<u>문자 없는 경우도 포함</u>) 모든 문자 데이터를 의미

- 와일드 카드 문자가 데이터 일부인 경우
```sql
-- ₩ 문자 바로 뒤에 있는 _ 는 와일드 카드 기호로서가 아닌 데이터에 포함된 문자로 인식해라
SELECT *
   FROM SOME_TABLE
 WHERE SOME_COLUMN LIKE 'A₩_A%' ESCAPE '₩';
```

#### IS NULL 연산자
- NULL : 데이터 값이 완전히 '비어 있는' 상태
```sql
-- 어떤 데이터도 나오지 않는 결과
SELECT *
   FROM EMP
 WHERE COMM = NULL;
```
> COMM 열 값이 NULL인 행이 나와야 할 것 같지만, 실제로 출력되는 데이터는 없다.      
> 왜냐, NULL은 산술 연산자와 비교 연산자로 비료해도 결과 값이 NULL이 되기 때문이다. WHERE절은 조건식의 결과값이 true인 행만 출력하는데 
> 이처럼 연산 결과 값이 NULL이 되어버리면 조건식의 결과값이 true도 false도 아니게 되므로 출력 대상에서 제외된다.     

COMM 열 값이 NULL인 데이터를 출력하고 싶다면 IS NULL 연산자를 사용하면 된다.
```SQL
-- 추가 수당 열 값이 NULL인 데이터 출력하기
SELECT *
   FROM EMP
 WHERE COMM IS NULL;

-- 직속 상관이 있는 사원 데이터만 출력하기
SELECT *
   FROM EMP
 WHERE MGR IS NOT NULL;
```
#### 집합 연산자
주의 : 집합 연산자로 두 개의 SELECT문의 결과 값을 연결할 때 각 SELECT 문이 출력하려는 열 개수와 각 열의 자료형이 순서별로 일치해야 한다.
```sql
-- 오류 뜨는 예 1 : 출력 열 개수가 다를 때
SELECT EMPNO, ENAME, SAL, DEPTNO
   FROM EMP
WHERE DEPTNO = 10
UNION
SELECT EMPNO, ENAME, SAL
   FROM EMP
WHERE DEPTNO = 20;

-- 오류 뜨는 예 2 : 출력 열의 자료형이 다를 때
SELECT EMPNO, ENAME, SAL, DEPTNO
   FROM EMP
WHERE DEPTNO = 10
UNION
SELECT ENAME, EMPNO, DEPTNO, SAL
   FROM EMP
WHERE DEPTNO = 20;
```
- 집합연산자의 종류
   - UNION : 연결된 SELECT문의 결과 값을 합집합으로 묶어 준다. 결과 값의 중복은 제거된다.
   - UNION ALL : 위와 같다. 다만, 중복된 결과 값도 제거 없이 모두 출력된다.
   - MINUS : 먼저 작성한 SELECT문의 결과 값에서 다음 SELECT문의 결과 값을 차집합 처리한다. 
   - INTERSECT : 먼저 작성한 SELECT문과 다음 SELECT문의 결과 값이 같은 데이터만 출력된다. 교집합과 같은 의미


#### 연산자 우선순위
```
산술연산자(*, / > +,-) > 대소비교 연산자(=, !=, ^=, <>, >, >=, <, <=) > (그 외)비교 연산자(IS NULL, LIKE, IN) > BETWEEN 연산자 > NOT > AND > OR
```

#### <연습문제>    
```sql
--Q1
SELECT *
  FROM EMP
WHERE ENAME LIKE '%S';
 
 
--Q2
SELECT EMPNO, ENAME, JOB, SAL, DEPTNO
  FROM EMP
WHERE DEPTNO = 30 AND JOB = 'SALESMAN';
 
 
--Q3
--i) 집합 연산자 사용하지 않은 방식
SELECT EMPNO, ENAME, JOB, SAL, DEPTNO
  FROM EMP
WHERE DEPTNO IN (20, 30) AND SAL >= 2000;
--ii) 집합 연산자 사용한 방식
SELECT EMPNO, ENAME, JOB, SAL, DEPTNO
   FROM EMP
WHERE DEPTNO = 20 AND SAL >= 2000
UNION
SELECT EMPNO, ENAME, JOB, SAL, DEPTNO
  FROM EMP
WHERE DEPTNO = 30 AND SAL >= 2000;


--Q4
SELECT * 
  FROM EMP
WHERE SAL < 2000 OR SAL > 3000;


--Q5
SELECT ENAME, EMPNO, SAL, DEPTNO
  FROM EMP
WHERE ENAME LIKE '%E%' AND DEPTNO = 30 AND SAL NOT BETWEEN 1000 AND 2000


--Q6
SELECT * 
  FROM EMP
WHERE COMM IS NULL AND MGR IS NOT NULL AND JOB IN ('MANAGER', 'CLERK') AND ENAME NOT LIKE '_L%';
```
