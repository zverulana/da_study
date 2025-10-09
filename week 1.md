<details>
  <summary>[1] JOIN'ы - дубли [BASE]</summary>

# [1] Задача
![альтернативный текст](https://github.com/Vershinin-Artem/treaning/blob/main/dop/Image.png?raw=true)

## Ответ:  
INNER JOIN вернет 2 строки  
LEFT JOIN вернет 3 строки  
FULL OUTER JOIN вернет 5 строк  

# [2] Задача
![альтернативный текст](https://github.com/Vershinin-Artem/treaning/blob/main/dop/image-25.png?raw=true)

Запрос 1 

| T1.t1 | T2.t2 |
|-------|-------|
| 1     | 1     |
| 1     | 1     |
| 2     | 2     |
| 2     | 2     |
| 2     | 2     |
| 2     | 2     |
| 3     | 3     |
| 4     | null  |
| null  | null  | 


Запрос 2  
| T1.t1 | T2.t2 |
|-------|-------|
| 1     | 1     |
| 1     | 1     |
| 2     | 2     |
| 2     | 2     |
| 2     | 2     |
| 2     | 2     |
| 3     | 3     | 

-- Был затуп в обоих запросах с null, разобрался что к чему

# [3] Задача

Даны две таблицы T1 (col1, col2) и T2 (col1, col3)

В таблице T1 три записи
В таблице T2 четыре записи

Оцените какое максимальное и минимальное количество строк вернут   
INNER - min(0), max(12)  
LEFT - min(3), max(12)  
FULL - min(4), max(12)  
CROSS JOIN - min(12), max(12) 

# [4] Задача
да

# [5] Задача

min(5), max(100) --маневр понятен, но внимательность конечно же подвела

# [6] Задача

min(0), max(1000)

# [7] Задача

1-1  
1-1  
2-2  
2-2  
3-null  
4-null  
5-null  
7-null  
9-9  
null-null  

# [8] Задача

```sql
SELECT name, COUNT(*)
FROM t1
LEFT JOIN t2 ON t1.id = t2.id
```


Запрос вернет 4 строки, так как при left join в нашем случае количество строк в таблице t1 не поменяется,  COUNT(*) посчитает все строки, даже null. В отличии от ситуации, если бы мы считали COUNT(t1.id), тогда бы мы получили 3 строки, так как Caunt по колонке не считает null

# [9] Задача
24 строки

# [10] Задача
просто значение 8 и вернет столько строк значения 8, сколько строк в нашей таблице

# [11] Задача
Да

# [12] Задача
inner

# [13] Задача
min(100), max(1000)

# [13] Задача
inner
# [14] Задача
Inner

| t1.a | t2.b |
|------|------|
| 1    | 1    |
| 1    | 1    |
| 2    | 2    |
| 2    | 2    |
| 2    | 2    |
| 2    | 2    |
| 4    | 4    |

LEFT
| t1.a | t2.b |
|------|------|
| 1    | 1    |
| 1    | 1    |
| 2    | 2    |
| 2    | 2    |
| 2    | 2    |
| 2    | 2    |
| null | null |
| 4    | 4    |
| 5    | null |
| null | null |

RIGHT
| t1.a | t2.b |
|------|------|
| 2    | 2    |
| 2    | 2    |
| 1    | 1    |
| null | 3    |
| 4    | 4    |
| null | null |
| 1    | 1    |
| 2    | 2    |
| 2    | 2    |


FULL
| t1.a | t2.b |
|------|------|
| 1    | 1    |
| 1    | 1    |
| 2    | 2    |
| 2    | 2    |
| 2    | 2    |
| 2    | 2    |
| 4    | 4    |
| null | null |
| 5    | null |
| null | null |
| null | 3    |
| null | null |

# [16] Задача
c where - min(0), max(5)
без where - min(5), max(20)

# [17] Задача

 left - 5 
 inner  - 4
 cross - 12

</details>

<details>
  <summary>[2] UNION [BASE]</summary>
  
# [1] Задача

 Ответ: 5 7 3 4
 
 SUM = 19,   count(tab) = 4,   count(*) = 4,   count(DISTINCT) = 4,   max(7),   min(3)

# [2] Задача

 1-1    
 null-1  
 
  </details>

<details>
  <summary>[3] LAG/LEAD [BASE]</summary>
  
  # [1] Задача

  ```sql
WITH date_table AS (
SELECT DATE_TRUNK('month', created_at) AS mnth
       COUNT(*) AS count_value
FROM table t1
GROUP BY DATE_TRUNK('month', created_at)
)

SELECT mnth,
       count_value,
       (count_value - LAG(count_value, 1, 0) OVER (ORDER BY mnth)) / LAG(count_value, 1, 0) OVER (ORDER BY mnth) *100 AS prcnt
FROM date_table 
```
# [2] Задача
```sql
with lag_date as (
select good_id,
       inventory_dt as dt,
       abs(inventory_cnt - (lag(inventory_cnt, 1, 0) over (partition by good_id order by inventory_dt))) as delta      
from  sandbox.inventory_Artem_Vershin1n)

select good_id,
       dt,
       delta
from lag_date
where delta != 0
```

# [3] Задача
 ```sql
WITH transaction_table AS(
SELECT customer_id, 
       transaction_id,
       amount_rur,
       LAG(amount_rur, 1, 0) OVER(PARTITION BY customer_id ORDER BY transaction_dttm) AS tr_lag,
       LEAD(amount_rur, 1, 0) OVER(PARTITION BY customer_id ORDER BY transaction_dttm) AS tr_lead
       
FROM transaction

),
CASE_table AS (
SELECT customer_id, 
       transaction_id,
       amount_rur,
       tr_lag,
       tr_lead,
       CASE
       WHEN amount_rur > tr_lag AND amount_rur > tr_lead THEN 'done'
       END AS status
FROM transaction_table 
)
SELECT transaction_id
FROM CASE_table 
WHERE status = 'done'
```
# [4] Задача
```sql
WITH t1 AS (
SELECT
	товар,
	дата с,
	LEAD(дата с) OVER(PARTITION BY товар ORDER BY дата с) AS lead_data,
	цена
FROM table
)
SELECT
	товар,
	дата с,
	lead_data - INTERVAL '1 day' AS дата по,
	цена
FROM t1
```
# [5] Задача
```sql
WITH users_min_zp_mmay AS (
SELECT EMPL_FIO,
       AMOUNT
FROM Salary
WHERE VALUE_DAY BETWEEN '2023-05-01' AND '2023-05-31'
ORDER BY AMOUNT
LIMIT 3
),

users_min_zp_aprl AS (
SELECT EMPL_FIO,
       AMOUNT 
FROM Salary
WHERE EMPL_FIO IN (SELECT EMPL_FIO FROM users_min_zp_mmay) AND VALUE_DAY BETWEEN '2023-04-01' AND '2023-04-30' 
) 
SELECT m.EMPL_FIO, m.AMOUNT - a.AMOUNT as delta
FROM users_min_zp_mmay m
JOIN users_min_zp_aprl a
USING(EMPL_FIO)
```
# [6] Задача
```sql

WITH t1 AS (
    SELECT 
	DATE_TRUNC('week', hitTime) AS week,
	COUNT(hitID) AS events_now_week
    FROM hits
    GROUP BY DATE_TRUNC('week', hitTime)
)
SELECT 
	week,
	events_now_week,
	LAG(events_now_week) OVER(ORDER BY week) AS events_previos_week
FROM t1
```
# [7] Задача
```sql
WITH t1 AS (
SELECT num,
       CASE
       WHEN num + 1 = LEAD(num, 1, 0) OVER(ORDER BY num)
       AND LAG(num, 1, 0) OVER(ORDER BY num) = 0
       AND num - 1 != LAG(num, 1, 0) OVER(ORDER BY num)
       THEN 'norm'
       END AS status           
FROM A
)

SELECT num
FROM t1
WHERE status = 'norm'
```
# [8] Задача
```sql
WITH t1 AS (
    SELECT
	some_id,
	LAG(some_id) OVER(ORDER BY some_id) AS some_lag
    FROM table
),

t2 AS (
    SELECT 
	some_lag + 1 AS start_win,
	some_id - 1 final_win	
	
	FROM t1
	WHERE some_lag IS NOT NULL AND some_id - some_lag > 1
)

SELECT 
	start_win, 
	final_win
FROM t2
WHERE final_win - start_win + 1 > 2
```
# [9] Задача
```sql
WITH counting_cl AS 
(
SELECT
     DATE_TRUNC('month', order_date) AS mnth,
     COUNT(DISTINCT client_id) AS client_value
FROM table
GROUP BY DATE_TRUNC('month', order_date)
)

SELECT 
mnth,
(client_value - LAG(client_value) OVER (ORDER BY mnth)) / LAG(client_value) OVER (ORDER BY mnth) * 100 AS prct
FROM counting_cl
```

 </details>
 
  <details>
<summary>[4] NULL'ы [UPPER]]</summary>

  # [1] Задача

так как все условия соединены оператором or, на выходе мы получим все строки, так как null is null валидное условие,  в отличие от остальных, так как null это что то неизвестное, по этому даже null = null невыполнимое условие.  


# [2] Задача
 ```sql

SELECT 
    i.article_id,
    a.category_id
FROM 
    art a
LEFT JOIN 
    ins i
ON i.article_id = a.id
WHERE  a.category_id IS NULL	  
 ```

# [3] Задача
 ```sql
WITH t1 AS (
SELECT
    a.id AS id
FROM
    ins i
JOIN 
    oplog o
ON i.id = o.instance_id
JOIN 
    art a
ON a.id = i.article_id
    
WHERE 
    DATE_PART('month', date_borrowed) IN (6, 7, 8) 
)

SELECT
    art.id
FROM art 
LEFT JOIN t1
ON art.id = t1.id
WHERE t1.id IS NULL	  
 ```

# [4] Задача
 ```sql

    SELECT
	c.name
    FROM
	client c
    LEFT JOIN
	account a ON c.id = a.client_id
    LEFT JOIN 
	transaction t ON account_id = a.id
    WHERE close_dt IS NULL AND a.open_dt <= CURRENT_DATE - INTERVAL '1 year'
     GROUP BY
	c.name
    HAVING
        SUM(t.amount) FILTER (WHERE t.transaction_date >= CURRENT_DATE - INTERVAL '1 month') < 5000	  
 ```

# [5] Задача
 ```sql
1 ------------

SELECT
    client_fio
FROM 
    clients
WHERE client_type = 'Пенсионер' AND client_adress NOT LIKE '%Москва%'

2 ------------

SELECT
    c.client_fio,
    d.document_no
FROM 
    clients c
LEFT JOIN
    documents d USING(client_id)
WHERE 
    valid_to = '2220-01-01'


3 ---------------

SELECT
    client_id
FROM 
    documents d1
LEFT JOIN 
    documents d2 ON d1.client_id = d2.client_id AND d1.valid_to = '2220-01-01'

WHERE 
    d2.client_id IS NULL


4 --------------

SELECT
    client_id
FROM 
    documents
WHERE 
    valid_to = '2220-01-01'
GROUP BY
    client_id
HAVING 
    COUNT(valid_to) > 1


5 ---------------
SELECT
    c.client_fio
FROM 
    clients c
LEFT JOIN
    documents d USING(client_id)
WHERE
    d.documents_type IS NULL
	  
 ```

# [6] Задача
 ```sql
SELECT
    d.name
FROM
    department d
JOIN 
    employee e USING (id)
WHERE 
    e.name NOT EXISTS (SELECT
                       name
                   FROM 
                       employee
                   WHERE 
                       UPPER(name) LIKE '%Z%W%') 	  
 ```

# [7] Задача

Первый запрос соединит таблицы по двум условиям из джоин, но по итогу оставит только те строки которые удволетворяют условие написанное в WHERE  
Второй запрос выведет все строки из левой таблицы + все строки удволетворяющие условиям.  
Если по каим то строкам не будет пересечения с правой таблицей то будет NULL   

# [8] Задача
 ```sql
SELECT c.fio
FROM clients c
JOIN credits cr USING(client_id)
WHERE cr.credit_id IS NOT NULL	  
 ```
# [9] Задача
 ```sql
null	  
 ```
# [11] Задача
 ```sql
1 ----------

1 A  1    A
2 B null null
3 C null null

2 ----------

1 A 1 A	  
 ```

# [10] Задача
 ```sql
нет, это обычная сортировка, которая может изменить только порядок записей.	  
 ```

 </details>

<details>
  <summary>[5] JOIN'ы - минус один в секции ON + нечеткие JOIN'ы [UPPER]</summary>

   # [1] Задача
 ```sql
1 ------
WITH cte1 AS (
    SELECT
        DATE_TRUNC('month', date) AS month,
        user_id
    FROM
        logins
    GROUP BY
        DATE_TRUNC('month', date), user_id
)

SELECT
    COUNT(DISTINCT cte1.user_id) AS uniq_users_retained
FROM
    cte1
JOIN
    cte1 AS cte2
ON
    cte1.user_id = cte2.user_id
    AND cte1.month = cte2.month - INTERVAL '1 month'

 ```

# [2] Задача
 ```sql
SELECT
    p.prod_cat,
    SUM(s.price * s.cnt) AS total_amt
FROM
    product p
JOIN 
    sale s
ON
    p.product_nm = s.product_nm AND s.sale_dt BETWEEN p.dt_from AND p.dt_to
GROUP BY
    prod_cat	  
 ```

# [3] Задача
 ```sql
SELECT
    p.prod_cat,
    SUM(s.price * s.cnt) AS total_amt
FROM
    product p
JOIN 
    sale s
ON
    p.product_nm = s.product_nm AND s.sale_dt BETWEEN p.dt_from AND p.dt_to
GROUP BY
    prod_cat	  	  
 ```

</details>
 
<details>
	
  <summary>[6] FIRST_VALUE/LAST_VALUE [UPPER]</summary>
  
  # [1] Задача
  
```sql
  
WITH case_table AS (
SELECT
    product_id,
    LAST_VALUE(dt) OVER (PARTITION BY product_id ORDER BY dt ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS LAST_v,
    CASE
    WHEN FIRST_VALUE(dt) OVER (PARTITION BY product_id ORDER BY dt) 
    != LAST_VALUE(dt) OVER (PARTITION BY product_id ORDER BY dt ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
    THEN 1
    ELSE 0
    END AS status
FROM fact_purchases
WHERE DATE_TRUNC('month', dt) = '2023-05-01'
)
SELECT product_id, LAST_v
FROM case_table
WHERE status = 1
ORDER BY product_id, LAST_v DESC

```
 </details>
 
<details>
  <summary>[7] GROUP BY [BASE]</summary>

# [1] Задача
```sql
SELECT user_id
FROM orders
GROUP BY user_id
HAVING COUNT(DISTINCT DATE_TRUNC('month', order_date)) = 6
```

# [2] Задача
```sql
SELECT c.name
FROM Cat c
JOIN Art a
ON c.id = a.category_id
GROUP BY c.name
HAVING AVG(a.deposit_price) > 5000
```
# [3] Задача
```sql
SELECT 
    date, 
    COUNT(DISTINCT user_id) AS DAU,
    COUNT(video_id) AS views
FROM reports_stg.watch_content
GROUP BY date
```
# [4] Задача
```sql
SELECT 
    a.tariff,
    COUNT(a.abonent_id) AS users,
    AVG(all_clc) FILTER (WHERE c.table_business_month = '2019-05-01') AS month_avg_balanc,
    AVG(data_traffic_GB) FILTER (WHERE t.table_business_month = '2019-04-01') AS mont_avg_traffic
FROM abnt a
JOIN clcd c USING(abonent_id)
JOIN traf t USING(abonent_id)
GROUP BY a.tariff
```
# [5] Задача

GROUP BY

# [6] Задача

```sql
1 -------

SELECT 
    o.order_date,
    p.name,
    COUNT(o.paynent_day) AS paynent_orders
FROM orders o
LEFT JOIN platforms p USING(platform)
GROUP BY  o.order_date,
          p.name

2 ------

SELECT 
    experience_id,
    COUNT(order_id) AS top
FROM orders
GROUP BY experience_id
ORDER BY top DESC
LIMIT 10

3 ------

SELECT
    user_id
FROM orders
WHERE order_status = 'Забронировано'
GROUP BY user_id
HAVING COUNT(users_id) > 1
```
# [7] Задача
```sql
SELECT f.client_id
FROM fct_sales f
JOIN dim_sku s USING(sku_id)
WHERE dttm::date >= CURRENT_DATE - INTERVAL '1 month'
GROUP BY f.client_id
HAVING SUM(s.weight * f.sku_cnt) > 2
```
# [8] Задача
```sql
SELECT user_id
FROM orders
GROUP BY user_id
HAVING COUNT(DISTINCT DATE_TRUNC('month', order_date)) = 6
```
# [9] Задача
```sql
WITH t1 AS (
SELECT
    COUNT(DISTINCT product_category) AS catg_value
FROM products
)
SELECT customer_id
FROM sales s
JOIN products p USING(product_id)
GROUP BY customer_id
HAVING COUNT(DISTINCT product_category) = (SELECT catg_value FROM t1)
```
# [10] Задача
```sql
SELECT 
    p.name,
    SUM(o.order_sum)
FROM orders o
LEFT JOIN programs p
ON o.program_id = p.id AND o.state = 'success'
GROUP BY p.name

```
# [11] Задача
```sql
SELECT program_id
FROM orders o
JOIN programs p
ON o.program_id = p.id
WHERE p.direction = 'Аналитика' 
AND state = 'success'
AND order_date::date BETWEEN '2021-10-01' AND '2021-12-31'
GROUP BY program_id
HAVING SUM(order_sum) >= 1000000
```
# [12] Задача
```sql
200 строк
```
# [13] Задача
```sql
1 строка с максимальным значением
```
# [14] Задача
```sql
50 строк 
```
# [15] Задача
```sql
select
    d.department_id,
    d.department_name
from department_tb d
left join employee_tb e USING(department_id)
group by d.department_id
having count(distinct employee_id) > 10
order by count(distinct employee_id) desc,
d.department_id
```
# [16] Задача
```sql
COUNT(A) - 2 строки
COUNT(B) - 3 строки
COUNT(DISTINCT A) - 1 строкa
SUM(A) - 2
SUM(B) - 3
SUM(A+B) - 4 
```
# [17] Задача
```sql
1 -----
SELECT
   MIN(A),
   MIN(B),
   MAX(A), 
   MAX(B)
FROM table_1 
CROSS JOIN  table_2


2----

SELECT *
FROM 
(
SELECT
   MIN(A), 
   MAX(A),
   SUM(A),
   COUNT(*)
FROM table_1) AS q1
CROSS JOIN
(
SELECT
   MIN(A), 
   MAX(A),
   SUM(A),
   COUNT(*)
FROM table_2) AS q2
```
# [18] Задача
```sql
SELECT employee_id
FROM table
WHERE DATE_PART('year', date_start) = 2024
GROUP BY employee_id
HAVING COUNT(date_start) >= 2
```
# [19] Задача
```sql
SELECT COUNT(student) as count
FROM Student_in_class
WHERE class = (
		SELECT id
		FROM Class
		WHERE name = '10 B'
	)
```
# [20] Задача
```sql

SELECT 
    date::date,
    COUNT(action) FILTER (WHERE action = 'accepted') * 1.0 /
    COUNT(action) FILTER (WHERE action = 'sent') AS cr

FROM fb_friend_requests
GROUP BY date::date
HAVING COUNT(action) FILTER (WHERE action = 'sent') >= 1 
AND COUNT(action) FILTER (WHERE action = 'accepted') >= 1
```
# [21] Задача
```sql
SELECT
MAX(num)
FROM
(
SELECT num
FROM MyNumbers
GROUP BY num
HAVING COUNT(*) = 1
) AS t1
```
# [22] Задача
```sql
1 ----

SELECT 
    date,
    ROUND(COUNT(action_type) FILTER (WHERE action_type = 'click') 
    / COUNT(action_type) FILTER (WHERE action_type = 'view') * 1.0, 2) AS cr 

FROM Events
WHERE widget_id = '522955' AND page_current_url = '/catalog'
GROUP BY date

2 ----

SELECT 
    date,
    ROUND(COUNT(action_type) FILTER (WHERE action_type = 'view') 
    / COUNT(action_type) FILTER (WHERE action_type = 'Page_view') * 1.0, 2) AS cr 
FROM Events
WHERE widget_id = '522957' AND page_current_url = '/catalog'
GROUP BY date
```
# [23] Задача
```sql
SELECT 
    store_id, 
    AVG(total_amount) AS AOV,
    COUNT(DISTINCT client_id) AS un_users
FROM transaction
GROUP BY store_id
```
# [24] Задача
1 - D  
2 - C  
3 - E  

 </details>


<details>
  <summary>[8] ROW_NUMBER [BASE]</summary>

# [1] Задача
```sql
WITH filtr_date AS (
    SELECT 
	id,
	adr,
	dt,
	ROW_NUMBER() OVER (PARTITION BY id ORDER BY dt) AS rn

FROM ATM
WHERE dt:: date = '2023-01-01'
)
SELECT
    id,
    adr,
    dt
FROM filtr_date
WHERE rn = 1
```

# [2] Задача
```sql
SELECT o.user_id, 
	a.model
FROM
    (
    SELECT 
	o.user_id, 
	a.model,
	o.date_borowwed,
	ROW_NUMBER() OVER(PARTITION BY o.user_id ORDER BY o.date_borowwed DESC) AS rn 
    FROM Oplog o
	JOIN Ias i ON o.instance_id = i.id
	JOIN Art a
	ON i.article_id = a.id
    GROUP BY user_id
    ) AS ranked
WHERE rn <= 3
```
# [3] Задача
```sql
WITH t1 AS (
    SELECT 
	transaction_id,
	customer_id,
	amount_rur,
	transaction_dttm,
	ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY transaction_dttm) AS rn,
	SUM(amount_rur) OVER(PARTITION BY customer_id) AS cumltv_sum
    FROM transaction
    WHERE success_flg = 1
)

SELECT 
    transaction_id,
    customer_id,
    amount_rur,
    transaction_dttm
FROM t1
WHERE rn = 1 AND cumltv_sum > 1000000
```
# [4] Задача
```sql
OVER
```
# [5] Задача
```sql
WITH t1 AS (
SELECT
    name,
    LEAD(name) OVER (PARTITION BY department_id ORDER BY salary) AS nxt_name,
    LEAD(salary) OVER (PARTITION BY department_id ORDER BY salary) AS nxt_salary,
    ROW_NUMBER() OVER (PARTITION BY department_id, salary ORDER BY id DESC) AS rn
FROM employee
)
SELECT * 
FROM t1 
WHERE rn = 1
```
# [6] Задача
```sql
1 -----
WITH data_rn AS (
SELECT 
    o.user_id,
    p.name,
    ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY buy_date) AS rn 
    
FROM orders o
LEFT JOIN programs p
ON o.program_id = p.id
)
SELECT 
    o.user_id,
    p.name,
FROM data_rn
WHERE rn = 1

2 -------

WITH data_rn AS (
SELECT 
    o.user_id,
    p.name,
    ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY buy_date) AS rn 
    
FROM orders o
LEFT JOIN programs p
ON o.program_id = p.id
)
SELECT 
    o.user_id,
    p.name,
FROM data_rn
WHERE rn = 3


3 -------

WITH data_rn AS (
SELECT 
    o.user_id,
    p.name,
    ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY buy_date) AS rn,
    COUNT(state) FILTER (WHERE state = 'success') OVER (PARTITION BY user_id) AS uc 
    
FROM orders o
LEFT JOIN programs p
ON o.program_id = p.id
GROUP BY user_id, p.name
)
SELECT
    o.user_id,
    p.name
FROM 
WHERE  (uc >= 3 AND rn = 3) OR (uc < 3 AND rn = 1)

```
# [7] Задача
```sql
1 ------
SELECT
    t.client_id,
    AVG(tb.amount) AS user_avg_value
FROM table_1 t
JOIN table_2 tb USING(card_id) 
WHERE DATE_PART('month', tb.date_transaction) = 5
GROUP BY t.client_id

2 -----

WITH t1 AS (
SELECT
    t.client_id,
    t.card_id,
    SUM(tb.amount) AS user_value
FROM table_1 t
JOIN table_2 tb USING(card_id) 
GROUP BY t.client_id, t.card_id
),
t2 AS (

SELECT
    *,
    ROW_NUMBER() OVER (PARTITION BY t.client_id, t.card_id ORDER BY user_value DESC) AS rn
FROM t1
)
SELECT
    t.client_id,
    t.card_id,
FROM t2
WHERE rn = 1

```
# [8] Задача
```sql
WITH t1 AS (
    SELECT
	DATE_TRUNC('month', date) AS mnth,
	product,
	category,
	SUM(amount) AS all_amount

    FROM table
    GROUP BY DATE_TRUNC('month', date), product, category)
t2 AS (
    SELECT 
	*, 
	ROW_NUMBER() 
	OVER(PARTITION BY mnth, product, category
	ORDER BY all_amount DESC) AS rn
    FROM t1)

SELECT *
FROM t2
WHERE rn <=5 


```
# [9] Задача
```sql
SELECT appn
FROM table
WHERE date_from >= '2019-03-01' AND date_to <= '2021-03-01'

```
# [10] Задача
```sql
WITH t1 AS (
SELECT
	id,
	msisdn,
	tarif_plan_id,
	account_id,
	raw_dt
	ROW_NUMBER() OVER(PARTITION BY id ORDER BY raw_dt DESC) AS rn
FROM abnt
)
SELECT
id,
msisdn,
tarif_plan_id,
account_id,
raw_dt
FROM t1
WHERE rn = 1
```

# [11] Задача
```sql
WITH t1 AS (
    SELECT 
	user_id,
	order_id,
	ROW_NUMBER() OVER(user_id ORDER BY order_timestamp) AS rn
    FROM orders
)
SELECT
user_id,
order_id
FROM t1
WHERE rn = 1
```
  </details>

  <details>
  <summary>[9] RANK / DENSE_RANK [BASE]</summary>

# [1] Задача
```sql
WITH t1 AS (
SELECT
    p.productname,
    COUNT(DISTINCT o.order_id) AS count,
    SUM(p.price) AS total_amount
FROM orders o
LEFT JOIN products p USING(product_id)
GROUP BY p.productname
)

SELECT
    productname,
    count,
    total_amount, 
    DENSE_RANK() OVER (ORDER BY count DESC) AS dr
FROM t1
ORDER BY count DESC
```
# [2] Задача
```sql
WITH rank_data AS (
SELECT 
    empl_fio,
    empl_dep
    DENSE_RANK() OVER (PARTITION BY empl_dep ORDER BY amount DESC) AS rank
FROM salary 
WHERE DATE_TRUNC('month', value_day) = '2023-04-01'
)
SELECT
    empl_fio,
    empl_dep
FROM rank_data
WHERE rank = 2
```

# [3] Задача
```sql
WITH t1 AS (
SELECT 
    col,
    DENSE_RANK() OVER(ORDER BY col DESC) AS rnk
FROM table
)
SELECT col
FROM t1
WHERE rnk = 2
```
# [4] Задача
```sql
WITH data_rank AS (
SELECT
    name,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank,
FROM table
),
rn_data AS (
SELECT
    name,   
    ROW_NUMBER() OVER(ORDER BY age) AS age_rank
FROM
    data_rank
WHERE salary_rank = 2 
)
 
SELECT 
    name
FROM rn_data 
WHERE age_rank = 1
```
# [5] Задача
```sql

SELECT 
    p.name,
    COUNT(order_id) AS orders_value
FROM orders o
JOIN programs p
GROUP BY p.name
WHERE order_date >= DATE_TRUNC('month', CURRENT_DATE)
ORDER BY orders_value DESC
LIMIT 5
```
# [6] Задача
```sql
WITH t1 AS (
SELECT 
    col,
    DENSE_RANK() OVER(ORDER BY col DESC) AS rnk
FROM table
)
SELECT col
FROM t1
WHERE rnk = 2
```
  </details>

  <details>
  <summary> [10] SUM + OVER [UPPER] </summary>

   # [1] Задача
 ```sql
1 -----------


SELECT
    transaction_date,
    SUM(transaction_sum) AS transaction_sum,
FROM 
    money
GROUP BY
    transaction_date


2 -----------

SELECT
    transaction_date,
    SUM(transaction_sum) OVER (ORDER BY transaction_date) AS c_transaction_sum,
FROM 
    money
GROUP BY
    transaction_date

3 -----------

WITH t1 AS (
    SELECT
        transaction_date,
        SUM(transaction_sum) AS transaction_sum,
    FROM 
        money
    GROUP BY
        transaction_date
)

SELECT
    transaction_date,
    SUM(transaction_sum) OVER () AS transaction_sum
FROM 
    t1	  
 ```

# [2] Задача
 ```sql
1)
WITH users_category AS (    
    SELECT
	user_id,
        COUNT(DISTINCT category) AS count_cat
    FROM 
	table
    GROUP BY
	user_id
),

t2 AS (
    SELECT
        category,
        user_id,
        SUM(amount) all_purch
    
    FROM 
        table
    WHERE 
        user_id IN (SELECT user_id 
                    FROM users_category 
                    WHERE count_cat IN (SELECT MAX(count_cat)    
                                        FROM users_category))
     GROUP BY 
        category,
	user_id
)   

SELECT
    category,
    user_id,
    all_purch / SUM(all_purch) OVER (PARTITION BY user_id) AS ptg

FROM 
    t2	  
 ```

# [3] Задача
 ```sql
SELECT
    client_id,
    dttm::date,
    SUM(f.sku_cnt * d.price) OVER (PARTITION BY client_id ORDER BY dttm::date) as c_SUM
FROM
    fct_sales f
JOIN 
    dim_sku d
ON
    d.sku_id = f.sku_id	  
 ```

# [4] Задача
 ```sql
WITH department_salary AS (
    SELECT
        d.name AS department_name,
        SUM(e.salary) AS department_salary
    FROM 
        employee e
    JOIN 
        department d ON e.department_id = d.id
    GROUP BY 
        d.name
)

SELECT
    department_name,
    department_salary,
    SUM(department_salary) OVER (ORDER BY department_salary) AS c_sum_salary_dep
FROM 
    department_salary	  
 ```

# [5] Задача
- Какие значения вы получите в столбце count_categories? Ответ: 3
  
- Какие значения вы получите в столбце new_amount? Ответ: одежда 100, обувь 60 и 40 

# [6] Задача
 ```sql
SELECT
    channel,
    gross_sales_mln,
    all_r * 100.0  / SUM(all_r) OVER() AS pctg
    
FROM
(

SELECT
    channel,
    (SUM(m.sold_quantity * p.gross_price)) / 1000000 AS gross_sales_mln,
     SUM(m.sold_quantity * p.gross_price) AS all_r    
FROM
    dim_customer c
JOIN
    fact_sales_monthly m
ON
    m.customer_code = c.customer_code AND m.fiscal_year = 2021
JOIN
    dim_gross_price p
ON
    p.product_code = m.product_code AND p.fiscal_year = 2021  

GROUP BY
    channel
) AS t1	  
 ```

# [7] Задача
 ```sql
1 --------------
SELECT
    a.an_name,
    a.an_price    
FROM 
    orders o
JOIN 
    analysis a
ON
    o.ord_an = a.id
WHERE ord_datetime::date BETWEEN '2024-08-05' AND '2024-08-05' + INTERVAL '1 week'

2 -----------

SELECT
    DATE_TRUNC('month', ord_datetime) AS date
    g.gr_name,
    g.gr_id,
    SUM(COUNT(o.ord_id)) OVER (PARTITION BY g.gr_name ORDER BY DATE_TRUNC('month', ord_datetime)) AS c_sum
FROM 
    orders o
JOIN 
    analysis a
ON
    o.ord_an = a.id
JOIN 
    groups g
ON
    g.gr_id = a.an_group
GROUP BY
    DATE_TRUNC('month', ord_datetime) AS date
    g.gr_name,
    g.gr_id	  
 ```

# [8] Задача
 ```sql
Задание 1
7

Задание 2
3 и 7

Задание 3
 7 - 1
 6 - 2
 3 - 3
 2 - 4

Задание 4

 3 40 
 6 10
 7 10
 NULL NULL

Задание 5
3
6
7
7
	  
 ```


</details>
  
  <details>
  <summary>[11] SELF JOIN [BASE]</summary>
	  
# [1] Задача
 ```sql
1 --------

SELECT 
    id,
    DENSУ_RANK() OVER (ORDER BY points DESC) AS rank
FROM competition
ORDER BY rank


2 ---------
SELECT
    c1.id,
    c1.points,
    COUNT(c2.id) + 1 AS rank
FROM competition c1
LEFT JOIN competition c2
    ON c1.points < c2.points
GROUP BY c1.id, c1.points
ORDER BY rank	  
 ```

# [2] Задача
 ```sql
SELECT e2.id
FROM employee e1
LEFT JOIN employee e2
ON e1.chief_flg = 1 
AND e2.chief_flg = 0 
AND e1.birth_dt  >  e2.birth_dt 
AND e1.department_id = e2.department_id	  
 ```

# [3] Задача
 ```sql
	  
 ```

# [4] Задача
 ```sql
Да	  
 ```

# [5] Задача
 ```sql
SELECT e2.id
FROM employee e1
LEFT JOIN employee e2
ON e1.chief_flg = 1 
AND e2.chief_flg = 0 
AND e1.birth_dt  >  e2.birth_dt 
AND e1.department_id = e2.department_id	  
	  
 ```


  </details>

  <details>
	  
  <summary> [12] Флажки [BASE] </summary>

  # [1] Задача
 ```sql
WITH t1 AS (
    SELECT
	fio	
    FROM 
	journal
    WHERE 
	mark = 5
    GROUP BY
	fio
    HAVING
	COUNT(mark) >= 10
),
t2 AS (
    SELECT
	fio,
	COUNT(mark) AS dvoyki
    FROM journal
    WHERE mark = 2 
    GROUP BY fio
)

SELECT
    fio,
    dvoyki
FROM t1
LEFT JOIN t2
USING (fio)	  
 ```

# [2] Задача
 ```sql
SELECT
    CASE
    WHEN distance_to_pump <= mpg * fuel_left
    THEN 'True'
    ELSE 'False'
    END AS status
FROM 
    table
	  
 ```

# [3] Задача
 ```sql
SELECT 
   DISTINCT user_id
FROM watch_content
WHERE user_id IN (SELECT user_id 
                  FROM watch_content 
                  WHERE video_id = 1)

AND user_id IN (SELECT user_id 
                  FROM watch_content 
                  WHERE video_id = 3)

AND user_id NOT IN (SELECT user_id 
                FROM watch_content 
                WHERE video_id = 2)	  
 ```

# [4] Задача
 ```sql

SELECT
    customer_rk,
    report_dt,
    SUM(transaction_rur_amt) FILTER (WHERE mcc_group_cd = 'Супермаркеты') AS tr_smarcet_rur_amt,
    SUM(transaction_rur_amt) FILTER (WHERE mcc_group_cd = 'Кафе и рестораны') AS tr_restaurant_rur_amt,
    SUM(transaction_rur_amt) FILTER (WHERE mcc_group_cd = 'АЗС') AS tr_fuel_rur_amt
    
FROM 
    table
GROUP BY 
    customer_rk,
    report_dt	  
 ```

# [5] Задача
 ```sql
1 ---------
SELECT
    DISTINCT customer_name
FROM 
    orders
WHERE
    customer_name IN (SELECT customer_name FROM orders WHERE prod_name = 'iPhone')
AND customer_name NOT IN (SELECT customer_name FROM orders WHERE prod_name = 'Airpods')

2 -----------

WITH t1 AS (
    SELECT 
        customer_name
    FROM 
        orders
    WHERE 
        prod_name = 'Airpods'

) 

SELECT
    customer_name
FROM 
    orders
WHERE 
    prod_name = 'iPhone' 
AND
    customer_name NOT IN (SELECT customer_name FROM t1)	  
 ```
  </details>
  
  <details>
    
  <summary> [13] Case when [UPPER]</summary>

# [1] Задача
 ```sql
SELECT
    CASE
    WHEN activation_date >= CURRENT_DATE - INTERVAL '7 day' THEN 'last_week'
    WHEN activation_date >= CURRENT_DATE - INTERVAL '14 day' THEN 'last_two_week'
    WHEN activation_date >= CURRENT_DATE - INTERVAL '1 month' THEN 'last_month'
    WHEN activation_date >= CURRENT_DATE - INTERVAL '6 month' THEN 'last_six_month'
    ELSE 'more_six_month'
    END AS status

FROM abnt	  
 ```

</details>
  
  <details>
    
  <summary> [14] Подзапросы [BASE]]</summary>

   # [1] Задача
 ```sql
SELECT
    name
FROM
    employee
WHERE salary = (SELECT MAX(salary) FROM employee)	  
 ```

# [2] Задача
 ```sql
SELECT
    d.name,
    e.name
FROM 
    employee e
JOIN
    department d
ON
    e.department_id = d.id
WHERE
    e.id = (SELECT 
                MAX(ee.id)
            FROM
                employee ee
            WHERE
                e.department_id = ee.department_id)
          	  
 ```

# [3] Задача
 ```sql
WITH t1 AS (
    SELECT
	account_id,
	MAX(t_date)
    FROM table
)
SELECT
    account_id,
    sum_val
FROM 
    table
WHERE 
    t_date <= '2025-05-04'
AND
    (account_id, t_date) IN t1	  
 ```

# [4] Задача
 ```sql
SELECT
    name
FROM 
    passenger
WHERE 
    LENGTH(name) IN (SELECT MAX(LENGTH(name)) FROM passenger)	  
 ```

# [5] Задача
 ```sql
SELECT
    MAX(num) AS max_unique
FROM 
    MyNumbers
WHERE num ON (SELECT
                  num
              FROM 
                  MyNumbers
              GROUP BY 
                  num
              HAVING 
                  COUNT(num) = 1)	  
 ```

</details>



