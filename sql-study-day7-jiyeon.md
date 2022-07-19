### 6-1 오라클 함수
오라클 함수는 함수 제작 주체를 기준으로 오라클 기본제공 **내장함수**와 사용자가 필요에 의해 직접 정의한 **사용자 정의 함수**로 나뉜다.
#### 내장 함수의 종류
- 단일행 함수 : 입력된 한 행당 결과가 하나씩 나오는 함수    
- 다중행 함수 : 여러 행이 입력되어 하나의 행으로 결과가 반환되는 함수     

### 6-2 문자 데이터를 가공하는 문자함수
> 대소문자를 바꿔주는 UPPER, LOWER, INITCAP 함수
- UPPER(문자열) : 괄호 안 문자 데이터를 모두 대문자로 변환하여 반환    
- LOWER(문자열) : 괄호 안 문자 데이터를 모두 소문자로 변환하여 반환
- INITCAP(문자열) : 괄호 안 문자 데이터 중 첫 글자는 대문자로, 나머지 문자는 소문자로 변환 후 반환   
```sql
--문자열 비교에 유용하게 사용할 수 있다
SELECT *
  FROM EMP
WHERE UPPER(ENAME) = UPPER('scott');

--사원 이름이 대문자로 출력되도록!
SELECT * UPPER(ENAME)
  FROM EMP;
```
> 문자열 길이를 구하는 LENGTH 함수
```sql
--이름의 길이 출력하기
SELECT ENAME, LENGTH(ENAME)
  FROM EMP;

--사원 이름의 길이가 5 이상인 행 출력하기
SELECT ENAME, LENGTH(ENAME)
  FROM EMP
WHERE LENGTH(ENAME) >= 5;

--바이트 수를 반환하는 LENGTHB 함수. 한글 1글자에 2byte이기 때문에 4byte가 출력됨
SELECT LENGTH('한글'), LENGTHB('한글')
  FROM DUAL;
  
--직책 이름이 6글자 이상인 데이터만 출력해보자
SELECT *
  FROM EMP
WHERE LENGTH(JOB) >= 6;
```

> 문자열 일부를 추출하는 SUBSTR 함수
- SUBSTR(문자열 데이터, 시작 위치, 추출 길이) : 문자열 데이터의 시작 위치부터 추출 길이만큼 추출.
- SUBSTR(문자열 데이터, 시작 위치) : 문자열 데이터의 끝까지 추출.
  - 시작 위치가 음수일 경우에는 마지막 위치부터 거슬러 올라간 위치에서 끝까지 추출
```sql
-- EMP 테이블의 모든 사원 이름을 세번째 글자부터 끝까지 출력
SELECT SUBSTR(ENAME, 3)
  FROM EMP;
  
-- 아래 세 개는 결과같이 같다
SELECT SUBSTR(JOB, 1)
  FROM EMP;

SELECT SUBSTR(JOB, 1, LENGTH(JOB))
  FROM EMP;
  
SELECT SUBSTR(JOB, -LENGTH(JOB))
  FROM EMP;
```

```sql
```

```sql
```


### 6-3 숫자 데이터를 연산하고 수치를 조정하는 숫자 함수



```sql
```

```sql
```

