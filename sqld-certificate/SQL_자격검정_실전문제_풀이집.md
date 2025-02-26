1. 2장 28번 (order by에 case문이 존재할 경우)<br>
ex) order by (case when id = 999 then 0 else id end) => id가 999라면 0으로 취급하고 id를 기준으로 오름차순

2. 2장 29번<br>
(3) select 절에 기술되지 않는 칼럼을 이후에 수행되는 order by 절에 사용하는 것은 논리적으로 맞지 않다.<br>
(4) order by에 집계함수를 사용해도 상관없음

3. 2장 31번<br>
(1) COALESCE(칼럼1, 칼럼2, 칼럼3, ... , 칼럼 N, ...) => 첫 번째 인자부터 순서대로 확인하여 null이 아닐 때, 해당 칼럼(값)을 반환<br>
(2) NULLIF(칼럼1, ‘a’) => 칼럼1과 ‘a’가 같으면 null 반환, 다르면 칼럼1 반환<br>
(3,4) → NVL(칼럼1, ‘a’) =>  칼럼1이 null일 때, ‘a’ 반환 (= IFNULL(칼럼1, ‘a’), dbms 차이)

4. 2장 35번<br>
EQUI JOIN(등가 조인) : 두 개의 테이블 간에 칼럼 값들이 서로 정확하게 일치하는 경우에 사용되는 방법이다. “=” 연산자를 사용해서 표현한다.<br>
NON EQUI JOIN : “>”, “<”, “>=”, “<="<br><br>
(나) : 옵티마이저는 2개씩 묶어 join 수행

5. 2장 37번<br>
SELECT (σ) : 튜플 조회, 수평 연산, where절<br>
PROJECT (π) : 속성 조회, 수직 연산, select절<br>
JOIN (▷◁) : 공통 속성을 중심으로 2개의 릴레이션을 하나로 합쳐서 새로운 결과 릴레이션을 구성<br>
DIVISION(OR)DIVIDE (÷) : 릴레이션S의 속성값을 가지고 있는 릴레이션R의 튜플로 결과 릴레이션을 구성, 결과 릴레이션에는 릴레이션S의 속성은 없다.<br>

6. 2장 38번<br>
#custId# : 동적 변수 값

7. 2장 39번<br>
카테시안 곱 : 두 테이블 join 시, 적절한 join 조건이 없을 때 각 행에 대해 모든 행을 결합하게 되어 결과적으로 행의 갯수가 (행 갯수) x (행 갯수) 가 됨 (= cross join)

8. 2장 42번<br>
USING : on과 같이 join에 대한 조건을 설정할 때 사용, 연결하려는 칼럼이 같을 때 간단하게 표현 가능<br>
ex) USING (STADIUM_ID)<br>
USING (T.STADIUM_ID = S.STADIUM_ID) ← SYNTAX 에러 발생

9. 2장 43번<br>
join 조건을 설정하지 않으면 cross join이 됨<br><br>
(2) SELF JOIN : 나 자신의 테이블을 조인함<br>
(3) NATURAL JOIN : 두 테이블의 같은 칼럼을 찾아서 알아서 조인함 (on절 사용 x)

10. 2장 44번<br>
left outer join 이후에 inner join 조건에 해당하는 튜플이 없어도 null 값으로 조인

11. 2장 45번<br>
[UNION]<br>
-여러 쿼리문들을 합쳐서 하나의 쿼리문으로 만들어주는 방법<br>
-중복 값 제거<br>
[UNION ALL]<br>
-UNION과 같이 여러 쿼리문들을 합쳐서 하나의 쿼리문으로 만들어주는 방법<br>
-중복 값 허용<br>
[공통]<br>
칼럼명, 칼럼타입, 칼럼 갯수 모두 동일 해야함<br><br>
(다)<br>
첫 번째 select는 공통되는 튜플 조인(inner join),<br>
두 번째 select는 공통되지 않는 튜플 중 A.id만 출력(left outer join),<br>
세 번째 select는 공통되지 않는 튜플 중 B.id만 출력(right outer join) → full outer join

12. 2장 49번<br>
(+)의 의미는 (+) 표시 된 반대편 테이블을 기준으로 outer join 수행<br>
ex) A.id = B.id(+) (== A left outer join B on A.id = B.id)<br><br>
B.삭제여부가 where에 있으면 안되는 이유 => left outer join 이후, B.삭제여부 = ‘N’을 필터링할 경우 null값 까지 제외시키기 때문에

13. 2장 52번

[1] ROLLUP<br>
-소그룹 + 전체 합계 도출<br>
-맨 처음 명시한 컬럼에 대해서만 소그룹 합계를 구함<br>
```sql
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY ROLLUP(상품ID, 월);
```
![image](https://github.com/user-attachments/assets/c1de84b5-c6f7-4045-93bc-98bc94ad473a)


[2] CUBE<br>
-소그룹 + 전체 합계 도출<br>
-모든 컬럼에 대해 소그룹 합계 도출<br>
```sql
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY CUBE(상품ID, 월);
```
![image](https://github.com/user-attachments/assets/3021c449-83b0-40b9-90f9-eae3f3a7a77e)

[3] GROUPING SETS<br>
-소그룹 합계만 도출<br>
```sql
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY GROUPING SETS(상품ID, 월);
```
![image](https://github.com/user-attachments/assets/bddc5a3a-5fb7-4d63-9082-9abb2d1be6fe)

14. 2장 53번
    | **연산자** | **의 미** | **결   과** |
    | --- | --- | --- |
    | **UNION** | 합집합 | 중복을 제거한 결과의 합을 검색 |
    | **UNION ALL** | 합집합 | 중복을 포함한 결과의 합을 검색 |
    | **INTERSECT** | 교집합 | 양쪽 모두에서 포함된 행을 검색, 중복 제거 |
    | **MINUS, EXCEPT** | 차집합 | 첫 번째 검색 결과에서 두 번째 검색 결과를 제외한 나머지를 검색, 중복 제거 |

15. 2장 55번<br>
보기) 이용됐던 적이 있는 서비스의 데이터의 서비스ID, 서비스명, 서비스URL을 중복 없이 조회<br>
(1) 이용됐던 적이 있는 서비스를 조인하는 것은 맞지만 조회 시 중복된 결과가 나옴<br>
(2) minus 연산으로 이용됐던 적이 없는 서비스 데이터가 도출되고 not exists로 이용된 적 있는 서비스 데이터가 선택되어 보기의 sql과 같은 결과를 도출함<br>
(3) left outer join과 b.서비스ID가 null인 경우를 필터링하면 이용된 적 없는 서비스 데이터만 남게 됨<br>
(4) 서비스는 서비스 이용을 포함하는 관계이기 때문에 minus 연산을 수행하면 데이터가 남지 않게 됨

16. 2장 56번<br>
기존 테이블에 중복되는 값을 가지는 데이터가 이미 존재하더라도 union 연산 시, 해당 중복 값이 연산될 때 중복된 데이터가 사라진다.

17. 2장 58번<br>
OVER()<br>
-ORDER BY, GROUP BY 서브쿼리를 개선하기 위해 나온 함수, 집계 함수 뒤에 사용<br><br>
PARTITION BY<br>
-칼럼을 기준으로 그룹핑하여 집계 ex) SUM(칼럼2) OVER(PARTITION BY 칼럼1)<br><br>
RATIO_TO_REPORT()<br>
-그룹별 전체 값의 합계 대비 해당 행의 비율을 반환, OVER() 사용 가능, 0~1 사이 값으로 계산<br>
-모두 더하면 1<br>
ex) RATIO_TO_REPORT(salary) OVER()

18. 2장 60번<br>
계층적 데이터 쿼리<br><br>
[1] START WITH<br>
-계층 구조의 시작점을 지정한다.<br>
-ex) START WITH C2 IS NULL ⇒ C2가 NULL인 행에서 시작<br><br>
[2] CONNECT BY<br>
-부모와 자식 간의 관계를 지정한다.<br>
-PRIOR는 현재 레벨의 컬럼 값을 이전 레벨의 값과 비교하는 데 사용된다.<br>
-ex) CONNECT BY PRIOR C1 = C2 ⇒ 현재 행의 C1 값이 다음 행의 C2값과 일치하는 행을 찾는다.<br><br>
[3] ORDER SIBLINGS BY<br>
-같은 계층 레벨의 행을 정렬하는 데 사용된다.<br>
-ex) ORDER SIBLINGS BY C3 DESC ⇒ 같은 부모를 가진행(같은 계층)을 C3 값에 따라 내림차 순으로 정렬한다.<br><br>
결과)<br>
A<br>
C<br>
B<br>
D<br>
답) C

19. 2장 61번<br>
루트 노드의 LEVEL 값은 1이다.

20. 2장 64번<br>
CONNECT BY PRIOR C1 = C2<br>
-현재 행의 C1이 부모 행의 C2와 같을 때 연결<br><br>
CONNECT BY C1 = PRIOR C2<br>
-현재 행의 C2가 부모 행의 C1과 같을 때 연결<br><br>
LEVEL<br>
-오라클 계층 쿼리에서 계층 구조의 깊이를 나타내는 가상 컬럼<br>
-START WITH에서 시작한 루트 노드는 LEVEL = 1이고, 그 아래에 있는 자식들은 LEVEL = 2, 그다음 손자들은 LEVEL = 3...

21. 2장 66번<br>
Range Join 후, 조인 대상 테이블(B테이블) 칼럼을 기준으로 그룹핑하면 A의 다른 값에 의해 조인된 값이 쓰이기 때문에 기존 테이블(A테이블) 칼럼을 기준으로 그룹핑해야함

22. 2장 67번<br>
A||B = ‘AB’

23. 2장 68번
    | 서브쿼리 유형 | 행 수 | 컬럼 수 | 주요 연산자 | 특징 |
    | --- | --- | --- | --- | --- |
    | 단일 행 & 단일 컬럼 | 1개 | 1개 | `=, <, <=, >, >=, <>` |  |
    | 단일 행 & 다중 컬럼 | 1개 | 여러 개 | `(col1, col2) = (val1, val2)` | 메인 쿼리와 서브쿼리에서 비교하고자 하는 칼럼 개수, 위치가 동일해야함 |
    | 다중 행 & 단일 컬럼 | 여러 개 | 1개 | `IN`, `ANY`, `ALL` |  |
    | 다중 행 & 다중 컬럼 | 여러 개 | 여러 개 | `EXISTS`, `IN ((val1, val2), (val3, val4))` | 메인 쿼리와 서브쿼리에서 비교하고자 하는 칼럼 개수, 위치가 동일해야함 |

24. 2장 69번<br>
PERCENT_RANK()
-정렬 칼럼을 기준으로 백분위로 순위를 매길 때 사용
-반환 값 = (그룹별 로우의 순위 - 1) / (그룹별 전체 로우수 - 1)
-0 ≤ n ≤ 1 값 도출
-오름차순일 때 하위 백분위, 내림차순일 때 상위 백분위<br><br>
CUME_DIST()<br>
-해당 그룹별로 값의 **순위에 대한** 상대적인 **누적** 분포도를 반환<br>
-반환 값 = (그룹별 로우의 순위) / (그룹별 전체 로우수)<br>
-0 < n ≤ 1 값 도출<br>
-오름차순일 때 하위 백분위, 내림차순일 때 상위 백분위<br><br>
RANK()<br>
-정렬한 칼럼을 기준으로 순번을 매김<br>
-중복 순위를 고려하여 다음 숫자를 매김 ( ex) 1, 1, 3, 4… )<br><br>
DENSE_RANK()<br>
-정렬한 칼럼을 기준으로 순번을 매김<br>
-중복 순위 상관없이 다음 순위의 숫자를 매김 ( ex) 1, 1, 2, 3… )<br>
-dense : 밀집한<br><br>
NTILE()<br>
-행 데이터를 그룹별로 나누어 차례대로 행 번호를 부여하는 분석 함수<br>
-전체 행 데이터 수를 그룹으로 나누었을 때 나머지가 존재하면 첫 번째 그룹부터 나머지가 안남을 때까지 1씩 부여

![image (1)](https://github.com/user-attachments/assets/8e52db6b-c72d-40d3-b17e-3e00730d5912)


```sql
select empno, ename, sal 
	,NTILE(5) over (order by sal desc) as X
from emp;
```

행 갯수를 5를 나눴을 때 그룹 단위로 1부터 순위를 매기고 남는 행이 존재하면 남는 행 갯수 만큼 첫 번째 그룹부터 포함하여 순위를 매긴다.

25. 2장 71번
    
    ```sql
    SELECT A.MemberID, A.MemberName, A.Email 
    FROM Member A 
    WHERE EXISTS (
        SELECT 'X'
        FROM Event B, EmailLog C
        WHERE B.StartDate >= '2014-10-01'
        AND B.EventID = C.EventID
        AND A.MemberID = C.MemberID
        HAVING COUNT(*) < (
            SELECT COUNT(*) 
            FROM Event 
            WHERE StartDate >= '2014-10-01'
        )
    )
    
    -- 첫 번째 서브쿼리가 where 절에 있기 때문에 
    -- Member 테이블의 데이터가 한 번에 조인되는게 아니라
    -- 각 행의 데이터에 대해 조인이 이루어지기 때문에 
    -- 2014-10-01을 포함하여 이후의 회원별 메일발송 갯수와 이벤트 갯수를 비교할 수 있는 것이다.
    ```
    
    (4) HAVING은 GROUP BY 없이 집계 함수의 조건을 설정하기 위해 사용할 수 있다. 테이블 전체를 하나의 그룹으로 생각한다.
    
26. 2장 72번<br>
연관 서브쿼리<br>
-서브쿼리가 메인쿼리 칼럼을 가지고 있는 형태<br>
-메인쿼리의 데이터를 서브쿼리에서 조건이 맞는지 확인하고자 할 때 사용<br><br>
비 연관 서브쿼리<br>
-서브쿼리가 메인쿼리 칼럼을 가지고 있지 않은 형태<br>
-메인 쿼리에 서브쿼리의 실행 결과 값을 제공

27. 2장 73번<br>
스칼라 서브쿼리(Scalar Subquery)<br>
-SELECT 절에 사용되는 서브쿼리<br>
-다중행 결과가 나오면 ORA-01427: single-row subquery returns more than one row" 라는 오류가 발생<br><br>
인라인뷰(Inline View) ( = 다이나믹뷰(Dynamic View) )<br>
-FROM 절에 사용되는 서브쿼리

28. 2장 77번<br>
ROW_NUMBER()<br>
-각 그룹 내에서 정해진 순서를 기준으로 1부터 순번을 매김<br><br>
ROWS BETWEEN 범위1 AND 범위2<br>
-ORDER BY 절에서 사용<br>
-현재 행을 기준으로 계산할 범위를 지정<br>
-UNBOUNDED PRECEDING => 시작 지점을 첫 번째 행으로 설정하여 모든 이전 행을 포함<br>
-CURRENT ROW => 현재 행을 포함<br>
-ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW => 첫 행부터 현재 행의 범위<br><br>
1.각 윈도우 함수로 생성된 칼럼은 서로 영향을 주지 않는다.(+기존 테이블 변화에도 영향 없음)<br>
2.MAX(연봉) OVER (ORDER BY 연봉 DESC ROWS CURRENT ROW) AS COL3 => 현재 행의 연봉만 참조하므로 자기자신의 연봉과 같음

29. 2장 78번<br>
GROUPING()
-GROUPING 함수는 직접 그룹별 집계를 구하지는 않지만 ROLLUP, CUBE, GROUPING SETS를 지원하는 역할을 함<br>
-집계가 계산된 결과에 GROUPING(표현식) = 1이 되며, 그 외에는 GROUPING(표현식) = 0이 됨<br><br>

ROLLUP(칼럼1, 칼럼2)에서 칼럼1은 총 소계를 집계하는 것을 제외하면 null 값이 존재하지 않음

30. 2장 79번<br>
ANY<br>
-다중 행 연산에 사용, 조건을 만족하는 값이 하나라도 있다면 결과를 리턴<br>
ALL<br>
-다중 행 연산에 사용, 모든 조건을 만족하는 결과를 리턴

31. 2장 80번<br>
GROUP BY ROLLUP(A, B)랑 GROUP BY A, ROLLUP(B)의 차이<br>
-GROUP BY ROLLUP(A, B) → 전체 집계 포함 ((NULL, NULL))<br>
-GROUP BY A, ROLLUP(B) → 각 A별로만 ROLLUP 수행 (전체 집계 없음)

32. 2장 83번<br>
GROUPING SETS()<br>
-각 인수별 집계함수를 구하는 함수
-인수들 간에는 평등한 관계이므로 인수의 순서가 바뀌어도 결과는 같음<br><br>
(1) => 각각 상품ID와 월을 기준으로 집계해야하므로 GROUPING SETS(상품ID, 월) 이여야함

33. 2장 84번<br>
윈도우 함수는 OVER 문구를 키워드로 필수 포함<br>
	```sql
	SELECT WINDOW_FUNCTION (ARGUMENTS) OVER 
	( [PARTITION BY 컬럼] [ORDER BY 컬럼] [WINDOWING 절] )
	FROM 테이블명 ;
	```

34. 2장 88번<br>
ROWS와 RANGE 차이<br>
![image](https://github.com/user-attachments/assets/e4df292d-61fd-4c89-81ea-0b5e45d54bb0)<br>
ROWS : 조회된 데이터를 물리적 위치(ROWNUM)로 구분하여 모든 행이 1개의 행으로 인식<br>
RANGE : ORDER BY 절에 명시된 칼럼으로 논리적인 행 집합을 구성하며, 집합으로 묶인 그룹을 1개의 행으로 인식<br><br>
1 PRECENDING => 현재 행을 기준으로 1개 이전 행<br>
1 FOLLOWING => 현재 행을 기준으로 1개 다음 행<br>
UNBOUNDED PRECENDING => 처음 행부터 현재 행까지<br>
UNBOUNDED FOLLOWING => 현재 행부터 마지막 행까지

35. 2장 90번<br>
LAG([칼럼], [N], [VAL])<br>
-그룹 내 이전 몇 번째 행의 칼럼 값을 가져옴<br>
-N(몇 번째), VAL(값이 없을 경우 기본 값)은 option<br>
LEAD([칼럼], [N], [VAL])<br>
-그룹 내 이후 몇 번째 행의 칼럼 값을 가져옴<br>
-마찬가지로 N(몇 번째), VAL(값이 없을 경우 기본 값)은 option

36. 2장 91번<br>
GRANT<br>
-사용자(USER)에게 권한 부여<br>
REVOKE<br>
-사용자(USER)에게 부여한 권한을 다시 회수<br><br>
(3) REVOKE 문을 사용하여 권한을 취소하면 권한을 취소당한 사용자가 WITH GRANT OPTION을 통해서 다른 사용자에게 허가했던 권한글까지 연쇄적으로 모두 취소된다.

37. 2장 92번<br>
CASCADE : 해당 스키마 소속 객체들이 있으면 모두 삭제<br>
RESTRICT : 해당 스키마 소속 객체들이 있으면 오류를 내고 종료 (기본값)

38. 2장 95번<br>
CUBE(A, B) = GROUPINGB SETS((A, B), A, B, ())

39. 2장 100번<br>
ORACLE<br>
-ALTER TABLE 테이블명 MODIFY (칼럼명1 데이터 유형 [DEFAULT 식] [NOT NULL], 칼럼명2 데이터 유형 ...)<br>
SQL Server<br>
-ALTER TABLE 테이블명 ALTER COLUMN 칼럼명 데이터 유형 [DEFAULT 식] [NOT NULL];<br><br>
-SQL Server에서는 여러개의 칼럼을 ALTER COLUMN으로 동시에 수정하는 구문은 지원하지 않는다.<br>
-NOT NULL 칼럼을 수정할 때 NOT NULL 구문을 명시하지 않으면 기존의 NOT NULL 제약조건이 NULL로 변경된다.
