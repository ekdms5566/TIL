## lv2

[동명 동물 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59041)

```sql
SELECT NAME, COUNT(*) AS COUNT FROM ANIMAL_INS
WHERE NAME IS NOT NULL
GROUP BY NAME HAVING COUNT(*) >= 2 ORDER BY NAME
```

COUNT는 \*가 아니면 NULL을 세지 않는다.

[중복 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/59408)

```sql
SELECT COUNT(DISTINCT NAME) AS count
FROM ANIMAL_INS
```

[이름에 el이 들어가는 동물 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59047)

동물 이름 중 이름에 'EL'이 들어가는 개의 아이디와 이름 조회

```sql
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS
WHERE ANIMAL_TYPE = 'DOG' AND NAME LIKE '%EL%'
ORDER BY NAME
```

[NULL 처리하기](https://school.programmers.co.kr/learn/courses/30/lessons/59410)

COALESCE 함수는 SQL에서 사용되는 함수로, 여러 개의 인자 중에서 NULL이 아닌 첫 번째 값을 반환하는 역할. (NULL을 다른 값으로 대체하고자 할 때 유용하게 사용)

```sql
SELECT ANIMAL_TYPE, COALESCE(NAME, 'No name') AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

[DATETIME에서 DATE로 형 변환](https://school.programmers.co.kr/learn/courses/30/lessons/59414)

SQL에서 날짜와 시간을 원하는 형식으로 변환하는 데 사용되는 함수
DATE_FORMAT(date_value, format_string)

```sql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS 날짜
FROM ANIMAL_INS ORDER BY ANIMAL_ID
```

[가격이 제일 비싼 식품의 정보 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131115)

내림차순 정렬 후, 맨 위의 값 출력

```sql
SELECT * FROM FOOD_PRODUCT ORDER BY PRICE DESC LIMIT 1;
```

[중성화 여부 파악하기](https://school.programmers.co.kr/learn/courses/30/lessons/59409)

```sql
-- CASE문 사용
SELECT ANIMAL_ID, NAME,
  CASE
    WHEN SEX_UPON_INTAKE LIKE '%Neutered%' OR SEX_UPON_INTAKE LIKE '%Spayed%' THEN 'O'
    ELSE 'X'
  END AS 중성화
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

```sql
-- 만약 "Intact"가 SEX_UPON_INTAKE에 포함되어 있다면, LOCATE 함수는 'X'를 반환하고, 그렇지 않으면 'O' 반환
SELECT ANIMAL_ID, NAME,
if(LOCATE("Intact", SEX_UPON_INTAKE), "X", "O") AS 중성화
FROM ANIMAL_INS
```

```sql
-- "Neutered" 또는 "Spayed"라는 문자열을 정규 표현식으로 찾아서 해당하는 경우에는 "O"를, 그렇지 않은 경우에는 "X"를 반환
SELECT
  ANIMAL_ID,
  NAME,
  IF(SEX_UPON_INTAKE REGEXP ('Neutered|Spayed'),'O','X') AS 중성화
FROM
  ANIMAL_INS
ORDER BY
  ANIMAL_ID
```

[고양이와 개는 몇 마리 있을까](https://school.programmers.co.kr/learn/courses/30/lessons/59040)

동물 중 고양이와 개가 각각 몇 마리인지 조회, 고양이를 개보다 먼저 조회

```sql
SELECT ANIMAL_TYPE, COUNT(*) FROM ANIMAL_INS GROUP BY ANIMAL_TYPE ORDER BY ANIMAL_TYPE
```

```sql
SELECT
  ANIMAL_TYPE AS ANIMAL_TYPE,
  COUNT('*') AS count
FROM ANIMAL_INS
WHERE ANIMAL_TYPE = 'Cat' OR ANIMAL_TYPE = 'Dog'
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE
```

[카테고리 별 상품 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131529)

```sql
-- LEFT(PRODUCT_CODE, 2) 함수는 PRODUCT_CODE의 앞 2자리를 추출
SELECT LEFT(PRODUCT_CODE, 2) AS CATEGORY, COUNT(*) AS PRODUCTS
FROM PRODUCT GROUP BY LEFT(PRODUCT_CODE, 2)
ORDER BY PRODUCT_CODE
```

```sql
-- SUBSTR, GROUP BY, HAVING 사용
SELECT SUBSTR(PRODUCT_CODE, 1, 2) AS CATEGORY, COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY CATEGORY
  HAVING COUNT(CATEGORY)
ORDER BY CATEGORY;
```

[입양 시각 구하기(1)](https://school.programmers.co.kr/learn/courses/30/lessons/59412)

HOUR 함수 사용 / BETWEEN 사용

```sql
SELECT HOUR(DATETIME) AS HOUR, COUNT(*) AS COUNT
FROM ANIMAL_OUTS
WHERE HOUR(DATETIME) BETWEEN 9 AND 20
GROUP BY HOUR(DATETIME)
ORDER BY HOUR
```

```sql
SELECT hour(datetime) HOUR, count(*) COUNT
from animal_outs
group by hour(datetime)
having HOUR between 9 and 19
order by 1;
```

```sql
select date_format(datetime, '%H') as 'hour',count(animal_id) as 'count'
from animal_outs
group by hour
having hour >= 9 and hour <= 19
order by hour asc
```

[진료과별 총 예약 횟수 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/132202)

2022년 5월에 예약한 환자 수를 진료과코드 별로 조회, 5월예약건수 기준 정렬 후 같은 값이 있다면 진료과코드 기준 정렬

```sql
SELECT MCDP_CD AS '진료과코드', COUNT(*) AS '5월예약건수'
FROM APPOINTMENT
WHERE LEFT(APNT_YMD, 7)='2022-05'
GROUP BY MCDP_CD
-- 2번째 열 기준 정렬 → 1번째 열 기준 정렬
ORDER BY 2,1
```

```sql
-- YEAR 함수 or LIKE 함수 사용
SELECT MCDP_CD AS '진료과코드', COUNT(*) AS '5월예약건수'
FROM APPOINTMENT
WHERE YEAR(APNT_YMD)=2022 AND MONTH(APNT_YMD) = 5
-- WHERE APNT_YMD LIKE '2022-05%'
GROUP BY MCDP_CD
ORDER BY 2, 1;
```

[자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/151137)

3개(통풍시트, 열선시트, 가죽시트) 중 1개라도 포함된 종류별 자동차 수

```sql
-- WHERE LIKE 사용
SELECT CAR_TYPE, COUNT(*) AS CARS FROM CAR_RENTAL_COMPANY_CAR
WHERE OPTIONS LIKE '%통풍시트%' OR OPTIONS LIKE '%열선시트%' OR OPTIONS LIKE '%가죽시트%'
GROUP BY CAR_TYPE ORDER BY CAR_TYPE
```

```sql
-- 정규표현식 사용
SELECT
CAR_TYPE
,COUNT(car_id) AS cars
FROM CAR_RENTAL_COMPANY_CAR
WHERE REGEXP_LIKE(OPTIONS,'통풍시트|열선시트|가죽시트')
GROUP BY 1
ORDER BY 1
```

[상품 별 오프라인 매출 구하기 - JOIN](https://school.programmers.co.kr/learn/courses/30/lessons/131533)

PRODUCT 테이블과 OFFLINE_SALE 테이블에서 상품코드 별 매출액(판매가 \* 판매량) 합계를 출력
매출액을 기준으로 내림차순 정렬, 매출액이 같다면 상품코드를 기준으로 오름차순 정렬

```sql
SELECT A.PRODUCT_CODE, sum(B.SALES_AMOUNT * A.PRICE) AS SALES
FROM PRODUCT A
JOIN OFFLINE_SALE B ON A.PRODUCT_ID = B.PRODUCT_ID
GROUP BY 1
ORDER BY SALES DESC, A.PRODUCT_CODE
```

```sql
select p.product_code, sum(p.price * o.sales_amount) as sales
from product p
inner join offline_sale o on p.product_id = o.product_id
group by p.product_code
order by sales desc, p.product_code asc
```

```sql
SELECT P.PRODUCT_CODE, SUM(S.SALES_AMOUNT) * P.PRICE AS SALES
FROM PRODUCT P
inner JOIN OFFLINE_SALE S
ON P.PRODUCT_ID = S.PRODUCT_ID
GROUP BY P.PRODUCT_ID
ORDER BY SALES DESC, P.PRODUCT_CODE
```

[조건에 맞는 도서와 저자 리스트 출력하기 - JOIN](https://school.programmers.co.kr/learn/courses/30/lessons/144854)

```sql
SELECT B.BOOK_ID, A.AUTHOR_NAME, DATE_FORMAT(B.PUBLISHED_DATE, '%Y-%m-%d') AS PUBLISHED_DATE
FROM BOOK B
JOIN AUTHOR A ON B.AUTHOR_ID = A.AUTHOR_ID
WHERE B.CATEGORY = '경제'
ORDER BY PUBLISHED_DATE
```

```sql
-- 효율성을 위해 사전 필터링 WHERE 대신 ON 사용
SELECT BOOK_ID, AUTHOR_NAME, DATE_FORMAT(PUBLISHED_DATE, '%Y-%m-%d') as PUBLISHED_DATE
FROM BOOK
INNER JOIN AUTHOR
ON BOOK.CATEGORY = '경제' and BOOK.AUTHOR_ID = AUTHOR.AUTHOR_ID
ORDER BY PUBLISHED_DATE;
```

```sql
SELECT B.BOOK_ID, A.AUTHOR_NAME, DATE_FORMAT(B.PUBLISHED_DATE, '%Y-%m-%d') AS PUBLISHED_DATE
FROM BOOK B
  INNER JOIN AUTHOR A
  ON B.AUTHOR_ID = A.AUTHOR_ID
WHERE B.CATEGORY LIKE '경제'
ORDER BY B.PUBLISHED_DATE ASC;
```

[루시와 엘라 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59046)

이름이 Lucy, Ella, Pickle, Rogan, Sabrina, Mitty인 동물의 아이디와 이름, 성별 및 중성화 여부를 조회

```sql
-- WHERE IN 사용
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
```

```sql
-- 정규표현식 사용
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
WHERE NAME REGEXP '^(Lucy|Ella|Pickle|Rogan|Sabrina|Mitty)$'
ORDER BY ANIMAL_ID ASC
```

[성분으로 구분한 아이스크림 총 주문량 - GROUP BY](https://school.programmers.co.kr/learn/courses/30/lessons/133026)

각 아이스크림 성분 타입과 성분 타입에 대한 아이스크림의 총주문량을 총주문량이 작은 순서대로 조회

```sql
SELECT I.INGREDIENT_TYPE, SUM(TOTAL_ORDER) AS TOTAL_ORDER
FROM FIRST_HALF F
JOIN ICECREAM_INFO I ON F.FLAVOR = I.FLAVOR
GROUP BY I.INGREDIENT_TYPE
```

[가격대 별 상품 개수 구하기 - GROUP BY](https://school.programmers.co.kr/learn/courses/30/lessons/131530)

```sql
-- PRICE 열의 값을 1만 단위로 나눈 뒤 다시 1만을 곱하여 1만 단위로 묶는 연산을 수행
-- 가격이 1만 이하인 경우 0, 15000의 경우 10000, 25000의 경우 20000,
SELECT  PRICE DIV 10000 * 10000 AS PRICE_GROUP, COUNT(*) AS PRODUCTS FROM PRODUCT
GROUP BY PRICE_GROUP ORDER BY PRICE_GROUP
```

```sql
SELECT TRUNCATE(PRICE / 10000,0)*10000 "PRICE_GROUP", COUNT(PRICE) "PRODUCTS"
  FROM PRODUCT
  GROUP BY TRUNCATE(PRICE / 10000,0)*10000
  ORDER BY TRUNCATE(PRICE / 10000,0)*10000 ASC;
```

```sql
SELECT
  CASE
    WHEN price >= 10000 AND price < 20000 THEN 10000
    WHEN price >= 20000 AND price < 30000 THEN 20000
    WHEN price >= 30000 AND price < 40000 THEN 30000
    WHEN price >= 40000 AND price < 50000 THEN 40000
    WHEN price >= 50000 AND price < 60000 THEN 50000
    WHEN price >= 60000 AND price < 70000 THEN 60000
    WHEN price >= 70000 AND price < 80000 THEN 70000
    WHEN price >= 80000 AND price < 90000 THEN 80000
    END AS PRICE_GROUP
  ,COUNT(product_id) AS PRODUCTS
FROM PRODUCT
GROUP BY 1
ORDER BY 1
```

```sql
SELECT (price div 10000) * 10000 as PRICE_GROUP, count(*) as PRODUCTS
from product
group by PRICE_GROUP
order by PRICE_GROUP;
```

[3월에 태어난 여성 회원 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131120)

MEMBER_PROFILE 테이블에서 생일이 3월인 여성 회원의 ID, 이름, 성별, 생년월일을 조회하는 SQL문을 작성해주세요. 이때 전화번호가 NULL인 경우는 출력대상에서 제외시켜 주시고, 결과는 회원ID를 기준으로 오름차순 정렬해주세요.

```sql
SELECT MEMBER_ID, MEMBER_NAME, GENDER, DATE_FORMAT(DATE_OF_BIRTH, '%Y-%m-%d') AS DATE_OF_BIRTH
FROM MEMBER_PROFILE
WHERE GENDER='W' AND MONTH(DATE_OF_BIRTH) = 3 AND TLNO IS NOT NULL
ORDER BY MEMBER_ID
```

```sql
SELECT MEMBER_ID, MEMBER_NAME, GENDER, DATE_FORMAT(DATE_OF_BIRTH, "%Y-%m-%d")
FROM MEMBER_PROFILE
WHERE DATE_FORMAT(DATE_OF_BIRTH, '%m') = '03' AND GENDER = 'W' AND TLNO IS NOT NULL
ORDER BY MEMBER_ID
```

[재구매가 일어난 상품과 회원 리스트 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131536)

```sql
-- GROUP BY 1, 2 = 지정한 열의 순서를 인덱스로 사용하여 그룹화하는 것
SELECT USER_ID, PRODUCT_ID FROM ONLINE_SALE
GROUP BY 1, 2 HAVING COUNT(product_id) >= 2
ORDER BY 1, 2 DESC
```

```sql
SELECT USER_ID, PRODUCT_ID FROM ONLINE_SALE
GROUP BY USER_ID, PRODUCT_ID
HAVING COUNT(PRODUCT_ID) > 1
ORDER BY USER_ID, PRODUCT_ID DESC;
```

```sql
-- 서브 쿼리 사용
SELECT DISTINCT user_ID, PRODUCT_ID
FROM ONLINE_SALE
WHERE (user_ID, PRODUCT_ID) IN (
  SELECT user_ID, PRODUCT_ID
  FROM ONLINE_SALE
  GROUP BY user_ID, PRODUCT_ID
  HAVING COUNT(*) > 1
)
ORDER BY user_ID ASC, PRODUCT_ID DESC
```

[조건에 부합하는 중고거래 상태 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164672)

CASE WHEN 문법 익히기

```sql
SELECT BOARD_ID, WRITER_ID, TITLE, PRICE,
  CASE
    WHEN (STATUS = 'DONE') THEN '거래완료'
    WHEN (STATUS = 'SALE') THEN '판매중'
    ELSE '예약중'
  END AS STATUS
FROM USED_GOODS_BOARD
WHERE CREATED_DATE = '2022-10-05'
ORDER BY BOARD_ID DESC
```

```sql
SELECT BOARD_ID,
  WRITER_ID,
  TITLE,
  PRICE,
  CASE
    WHEN STATUS = 'SALE' THEN '판매중'
    WHEN STATUS = 'RESERVED' THEN '예약중'
    WHEN STATUS = 'DONE' THEN '거래완료'
    ELSE '준비중'
  END AS STATUS
FROM USED_GOODS_BOARD
WHERE TO_CHAR(CREATED_DATE, 'YYYY-MM-DD') = '2022-10-05'
ORDER BY BOARD_ID DESC
```

```sql
SELECT BOARD_ID, WRITER_ID, TITLE, PRICE,
  IF(STATUS='SALE','판매중',IF(STATUS='RESERVED','예약중','거래완료'))AS STATUS
FROM USED_GOODS_BOARD
WHERE CREATED_DATE = '2022-10-5'
ORDER BY BOARD_ID DESC
```

[자동차 평균 대여 기간 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/157342)

DATEDIFF(END_DATE, START_DATE): END_DATE와 START_DATE 사이의 날짜 차이를 계산
ROUND(AVG(DATEDIFF(END_DATE, START_DATE)) + 1, 1) : 계산된 평균 렌트 기간을 반올림하여 소수점 한 자리까지 표시

```sql
SELECT CAR_ID, ROUND(AVG(DATEDIFF(END_DATE,START_DATE)+1),1) AS AVERAGE_DURATION
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
HAVING AVERAGE_DURATION >= 7
ORDER BY AVERAGE_DURATION DESC, CAR_ID DESC
```

```sql
SELECT
  CAR_ID,
  ROUND(AVG(DATEDIFF(END_DATE,START_DATE)+1),1) AS AVERAGE_DURATION
FROM
  CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY
  CAR_ID
HAVING
  AVERAGE_DURATION>=7
ORDER BY
  AVERAGE_DURATION DESC,
  CAR_ID DESC
```

## lv3

## lv4

## lv5

[상품을 구매한 회원 비율 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131534)

```sql
SELECT
    YEAR(O.SALES_DATE) AS YEAR,
    MONTH(O.SALES_DATE) AS MONTH,
    COUNT(DISTINCT U.USER_ID) AS PUCHASED_USERS,
    ROUND(COUNT(DISTINCT U.USER_ID)/
        (SELECT COUNT(DISTINCT USER_ID) FROM USER_INFO
         WHERE YEAR(JOINED) = '2021') ,1) AS PUCHASED_RATIO
FROM USER_INFO U
LEFT OUTER JOIN ONLINE_SALE AS O ON U.USER_ID = O.USER_ID
WHERE YEAR(U.JOINED) = '2021' AND O.SALES_DATE IS NOT NULL
GROUP BY YEAR(O.SALES_DATE), MONTH(O.SALES_DATE)
ORDER BY 1, 2
```

```sql
SELECT
  YEAR(B.SALES_DATE) AS YEAR,
  MONTH(B.SALES_DATE) AS MONTH,
  COUNT(DISTINCT A.USER_ID) AS PUCHASED_USERS,
  ROUND(COUNT(DISTINCT A.USER_ID)/
    (SELECT COUNT(DISTINCT USER_ID) FROM USER_INFO
      WHERE YEAR(JOINED) = '2021') ,1) AS PUCHASED_RATIO
FROM USER_INFO A
LEFT OUTER JOIN ONLINE_SALE AS B ON A.USER_ID = B.USER_ID
WHERE
    YEAR(A.JOINED) = '2021' AND
    B.SALES_DATE IS NOT NULL
GROUP BY
    YEAR(B.SALES_DATE),
    MONTH(B.SALES_DATE)
ORDER BY
    YEAR,
    MONTH
```

```sql
SELECT SUBSTRING(OS.SALES_DATE,1,4) YEAR
  ,SUBSTRING(OS.SALES_DATE,6,2) MONTH
  ,COUNT(distinct OS.USER_ID) PUCHASED_USRES
  -- ,(SELECT COUNT(*) FROM USER_INFO) AS ALL_USERS
  ,ROUND(COUNT(distinct OS.USER_ID) / (SELECT COUNT(*) FROM USER_INFO WHERE JOINED BETWEEN '2021-01-01' and '2022-01-01' ),1) PUCHASED_RATIO
FROM USER_INFO UR
LEFT JOIN ONLINE_SALE OS ON UR.USER_ID = OS.USER_ID
WHERE UR.JOINED BETWEEN '2021-01-01' and '2022-01-01'
AND OS.SALES_DATE IS NOT NULL
GROUP BY SUBSTRING(OS.SALES_DATE,0,4)
    ,SUBSTRING(OS.SALES_DATE,6,2)
ORDER BY YEAR ASC, MONTH ASC
```

```sql
SELECT DATE_FORMAT(O.SALES_DATE, '%Y') AS YEAR,
        DATE_FORMAT(O.SALES_DATE, '%m') AS MONTH,
        COUNT(DISTINCT U.USER_ID) AS PUCHASED_USERS,
        ROUND(COUNT(DISTINCT U.USER_ID)/(SELECT COUNT(*) FROM USER_INFO WHERE joined LIKE '2021%'), 1) AS PUCHASED_RATIO
FROM USER_INFO U
JOIN ONLINE_SALE O
ON U.USER_ID = O.USER_ID
WHERE U.JOINED LIKE '2021%'
GROUP BY MONTH
ORDER BY YEAR, MONTH
;
```
