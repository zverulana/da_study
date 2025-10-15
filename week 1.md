<details>
  <summary>[1] JOIN'ы - дубли [BASE]</summary>

# [1] Задача

## Ответ:  
INNER JOIN: будет 2 записи
LEFT JOIN: будет 3 записи
FULL OUTER JOIN: будет 5 записей 

# [2] Задача

Запрос 1: 
t1   t2
1   1
1   1
2   2
2   2
2   2
2   2
3   3
4   NULL
NULL NULL


Запрос 2: 
t1   t2
1   1
1   1
2   2
2   2
2   2
2   2
3   3


# [3] Задача

                    INNER        LEFT        FULL        CROSS JOIN
Максимальное:        12           12          12            12
Минимальное:         0            3           4             12 

# [4] Задача
да

# [5] Задача

min: 10
max: 50

# [6] Задача

min: 0
max: 1000

# [7] Задача

t1   t2
1   1
1   1
2   2
2   2
3   NULL
4   NULL
5   NULL
7   NULL
9   9
NULL NULL  

# [8] Задача

```sql
SELECT name, count(1) FROM
t1 LEFT JOIN t2 USING (id)
GROUP BY name
```

Кол-во строк: 4

# [9] Задача
min: 24
max: 24

# [10] Задача
n раз 8, где n - кол-во строчек в таблице

# [11] Задача
да, результат может измениться, если таблицы не идентичные

# [12] Задача
INNER JOIN

# [13] Задача
min: 100
max: 1000

# [13] Задача
inner
# [14] Задача
INNER JOIN
a   b
1   1
1   1
2   2
2   2
2   2
2   2
4   4


LEFT JOIN 
1   1
1   1
2   2
2   2
2   2
2   2
4   4
null null
5  null
null null


RIGHT JOIN  
1   1
1   1
2   2
2   2
2   2
2   2
2   2
2   2
null 3
4   4
null null

FULL
1   1
1   1
2   2
2   2
2   2
2   2
4   4
5 null
null 3
null null
null null
null null

# [16] Задача
a) min:5   max:20
b) min:0   max:5

# [17] Задача
inner: 4
left: 5
right: 4
full: 5
cross: 12

</details>

<details>
  <summary>[2] UNION [BASE]</summary>
  
# [1] Задача

5
7
3
4

sum: 19
count: 4
count distinct: 4
max: 7
min: 3

# [2] Задача

1    1
null 1
 
  </details>

<details>
  <summary>[3] LAG/LEAD [BASE]</summary>
  
  # [1] Задача

```sql
SELECT dt, 
         count,
         CONCAT((count - LAG(count) OVER (ORDER BY dt)) / (LAG(count) OVER (ORDER BY dt)) * 100, '%') as Prcent_growth
FROM(
    SELECT date_trunc('month', created_at) as dt, 
           count (DISTINCT title) as count
    FROM table
    GROUP BY dt
    ) t1
ORDER BY dt;
```
 # [2] Задача
```sql
WITH t1 AS (SELECT good_id, inventory_dt,    
ABS (inventory_cnt - LAG(inventory_cnt, 1, 0) OVER (PARTITION BY good_id ORDER BY inventory_dt)) AS movement
FROM inventory
)

SELECT good_id, inventory_dt, movement as inventory_cnt
FROM t1
WHERE movement != 0;
```

  # [3] Задача
```sql
SELECT transaction_id
FROM transaction
WHERE amount_rur > LAG(amount_rur, 1, 0) OVER (PARTITION BY customer_id ORDER BY transaction_ddtt)
AND amount_rur > LEAD(amount_rur, 1, 0) OVER (PARTITION BY customer_id ORDER BY transaction_ddtt);
```

# [4] Задача
```sql
SELECT good_id, start_date,
CASE WHEN LEAD(start_date) OVER (PARTITION BY good_id ORDER BY start_date) IS NOT NULL
THEN LEAD(start_date) OVER (PARTITION BY good_id ORDER BY start_date) - INTERVAL '1 day'
ELSE '9999-12-31':: DATE
END AS date_finish,
price
FROM t1
```

# [5] Задача !!
```sql
with temp_1 as (SELECT * FROM(	
	SELECT distinct *,
   	 DENSE_RANK () OVER (ORDER BY amount) as rank
   	 FROM sandbox.salary
   	 WHERE VALUE_DAY between '2023-05-01' and '2023-05-31') t1
where rank <=3
order by rank),
diff as (
    select distinct *,
    amount - LAG(amount, 1, 0) OVER (PARTITION BY EMPL_FIO, EMPL_DEP
ORDER BY VALUE_DAY) as diff
    from sandbox.salary
)
select distinct empl_fio, amount, sum(diff) as diff from diff inner join temp_1 using (empl_fio, empl_dep, amount, value_day)
group by empl_fio, amount
order by amount;
```

# [6] Задача 
```sql
select hittime, 
actual, 
LAG(actual, 1, 0) OVER (order by hittime) as previos 
FROM(
	select  DATE(DATE_TRUNC('week',TO_TIMESTAMP(hittime))) as hittime, count(hitid) as actual
	from sandbox.hits 
	group by DATE_TRUNC('week',TO_TIMESTAMP(hittime))
	order by hittime
)
```

# [7] Задача 
```sql
WITH tmp AS( 
    SELECT num,
    LAG(num) OVER () as q1,
    LEAD(num) OVER () as q2
    FROM sandbox.num
)
SELECT num FROM tmp
WHERE num <> q1+1 AND num = q2-1
```
# [8] Задача 
```sql
select 
some_id - diff + 1 as start,
some_id - 1 as finish
FROM(
	select some_id,
	some_id - LAG(some_id, 1, 0) over (order by some_id) as diff
	from sandbox.some
)
where diff > 2
```
# [9] Задача 
```sql
WITH temp as (
SELECT DATE_TRUNC ('month', order_date) as month,
count(distinct client_id) as cnt
FROM table)

SELECT month,
CASE WHEN prev = 0 THEN NULL
ELSE (cnt-prev)/prev * 100
END as diff FROM
(SELECT *, LAG(cnt) OVER (ORDER BY month) as prev FROM temp)
```
 </details>
 
 <details>
  <summary>[4] NULL'ы [UPPER]</summary>

# [1] Задача 
Выведет все строки, потому что результатом where будет True (unknowm or true - это true)

# [2] Задача 
```sql
SELECT id 
FROM Art
WHERE id_category IS NULL
```
# [3] Задача 
```sql
table_summer as(
    select a.id, 1 as summer_flag
    from
    art a inner join oplog o USING(id)
    where extract(month from o.date_borrowed) in (6, 7, 8)
) 
select a.id from art left join table_summer
where summer_flag is null
```
# [4] Задача 
```sql
with money as (
	    select c.id as client_id, 
	    SUM(amount) as q1
	    from sandbox.a_client c INNER JOIN sandbox.a_account a ON c.id = a.client_id 
	    INNER JOIN sandbox.a_transaction t ON a.id = t.account_id and t.type = 'OUT'
	    WHERE transaction_date >= DATE_TRUNC('month', CURRENT_DATE - INTERVAL '1 month') and transaction_date < DATE_TRUNC('month', CURRENT_DATE)
	    group by c.id
	)
	SELECT distinct c.name FROM
	sandbox.a_client c INNER JOIN sandbox.a_account a ON c.id = a.client_id AND open_dt < CURRENT_DATE - INTERVAL '1 year' AND close_dt IS NULL
	INNER JOIN money ON money.client_id = c.id 
	WHERE money.q1 < 5000
```
# [5] Задача 
```sql
--1)
    SELECT Client_fio 
    FROM CLIENTS 
    WHERE Client_Type = 'Пенсионер' AND Client_Addres NOT LIKE $Москва$
-- 2)
    SELECT Client_fio, Document_no
    FROM CLIENTS INNER JOIN DOCUMENTS USING (Client_id)
    WHERE d.Valid_to = '2220-01-01'
--3)
    SELECT Client_fio
    FROM CLIENTS c LEFT JOIN DOCUMENTS d USING (Client_id) AND d.Valid_to = '2220-01-01'
    WHERE client_id IS NULL
--4)
SELECT Client_id
FROM DOCUMENTS
WHERE Valid_to = '2220-01-01'
GROUP BY Client_id
HAVING COUNT(*) > 1

--5)
SELECT Client_id
FROM CLIENTS c LEFT JOIN DOCUMENTS d USING (Client_id) 
WHERE Client_id IS NULL
```
# [6] Задача 
```sql
SELECT d.name FROM
DEPARTMENT d LEFT JOIN EMPLOYEE e ON d.id = e.department_id 
AND LOWER(e.name) LIKE '%z%' 
AND LOWER(e.name) LIKE '%w%'
WHERE e.name IS NULL
```
# [7] Задача 
В первом запросе сначала выполняется джойн, потом where, так что строки из первой таблицы удалятся. Во втором запросе все строки из первой таблицы останутся

# [8] Задача 
```sql
SELECT client_id FROM
clients cl LEFT JOIN credits cr USING (client_id)
WHERE cr.credit_id IS NULL
```
# [9] Задача 
5 + NULL будет NULL

# [10] Задача 
Может ли order by приводить к уменьшению строк? - нет

# [11] Задача 
1 запрос
____
1 A 1 A
2 B null null
3 C null null

2 запрос
____
1 A 1 A

   </details>

<details>
  <summary>[5] JOIN'ы - минус один в секции ON + нечеткие JOIN'ы [UPPER]</summary>

# [1] Задача 
```sql
WITH temp as(
    SELECT user_id, DATE_TRUNC('month', date) as month
    FROM logins
)
SELECT month, count(distinct user_id) as cnt
FROM temp t1 LEFT JOIN temp t2 ON t1.user_id = t2.user_id AND t1.month = DATE_TRUNC('month',t2.month-1)
GROUP BY month
```
# [2] Задача 
```sql
SELECT prod_cat,
SUM(price * cnt) as total_amt
FROM sale s LEFT JOIN product p ON s.prod_nm = p.prod_nm AND (sale_dt BETWEEN dt_from and dt_to) 
group by prod_cat
ORDER BY total_amt
```
# [3] Задача 
```sql
SELECT prod_cat,
SUM(price * cnt) as total_amt
FROM sale s LEFT JOIN product p ON s.prod_nm = p.prod_nm AND (sale_dt BETWEEN dt_from and dt_to) 
group by prod_cat
ORDER BY total_amt
```
# [4] Задача 
```sql
SELECT t1.a as color_1, t2.a as color_2
FROM 
a t1 CROSS JOIN a t2 ON t1.a < t2.a
ORDER BY color_1, color_2
```

</details>

<details>
  <summary>[6] FIRST_VALUE/LAST_VALUE [UPPER]</summary>

# [1] Задача 
```sql
WITH tmp AS(
    SELECT product_id,
    FIRST_VALUE(ds) OVER (PARTITION BY product_id ORDER BY ds) as first,
    LAST_VALUE(ds) OVER (PARTITION BY product_id ORDER BY ds RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) as last
    FROM fact_purchases
)
SELECT distinct product_id  from fact_purchases
WHERE ds BETWEEN '2023-01-05' AND '2023-05-31' AND first != last
ORDER BY product_id, last
```

  </details>

  </details>

<details>
  <summary>[7] GROUP BY [BASE]</summary>

# [1] Задача 
```sql
WITH tmp AS (SELECT user_id, count(DATE_TRUNC('month', date)) as month_cnt
FROM orders)
SELECT user_id FROM tmp
HAVING count(distinct month) = 6
```

# [2] Задача 
```sql
SELECT cat.name FROM category as cat
INNER JOIN Article as art ON cat.id = art.category_id
GROUP BY cat.name
HAVING avg(deposit_price) > 5000
ORDER BY cat.name
```

# [3] Задача 
```sql
SELECT date, count(*) as vid_count,
count(distinct user_id)
FROM table
GROUP BY date
ORDER BY date 
```

# [3] Задача 
```sql
-- 1
SELECT tariff, count(distinct abonent_id) FROM abnt 
GROUP BY tariff

-- 2
SELECT tariff, avg(all_clc) FROM abnt LEFT JOIN clcd USING (abonent_id)
GROUP BY tariff
WHERE table_business_month BETWEEN '2019-05-01' AND '2019-05-31'

-- 3
SELECT tariff, avg(data_traffic_gb) FROM abnt LEFT JOIN traf USING (abonent_id)
GROUP BY tariff
WHERE table_business_month BETWEEN '2019-04-01' AND '2019-04-31'

```
# [5] Задача 
Как можно получить уникальные значения без использования SELECT DISTINCT?
Через count(*), а потом HAVING count = 1
ИЛИ ROW_NUMBER

# [6] Задача 
```sql
-- 1
SELECT
DATE_TRUNC('day', order_date) as order_date,
name,
COUNT(payment_date) as cnt
FROM orders LEFT JOIN platforms USING (platform)
GROUP BY order_date, name

-- 2
SELECT distinct expiriens_id, count(distinct order_id) as cnt
FROM orders
GROUP BY expiriens_id
ORDER BY count(distinct order_id) DESC
LIMIT 5

-- 3
SELECT user_id
FROM orders
WHERE order_status = 'Забронировано'
GROUP BY user_id
HAVING count(distinct order_id) > 1 
```

# [7] Задача 
```sql
SELECT client_id FROM (
SELECT client_id, sum(sku_cnt * weight) as s
FROM fct_sales INNER JOIN dim_sku USING (sku_id)
GROUP BY client_id
WHERE dttm >= CURRENT_DATE - INTERVAL '1 month'
  AND dttm < CURRENT_DATE)
WHERE s > 2
```
# [8] Задача 
```sql
WITH tmp AS (SELECT user_id,
DATE_TRUNC ('month', order_date) as date
FROM orders)

SELECT user_id
FROM tmp
GROUP BY user_id
HAVING count(distinct date) = 6

```

# [9] Задача 
```sql
SELECT customer_id
FROM
sales INNER JOIN products USING (product_id)
GROUP BY customer_id
HAVING count(distinct product_category) = 
(select count(distinct product_category from products))

```

# [10] Задача 
```sql
SELECT name, COALESLE(sum(order_sum), 0) as s
FROM programs p LEFT JOIN orders o ON p.id = o.program_id AND o.state = 'success' 
GROUP BY name
ORDER BY s DESC
```

# [11] Задача 
```sql
SELECT name, COALESLE(sum(order_sum), 0) as s
FROM programs p LEFT JOIN orders o ON p.id = o.program_id 
AND o.state = 'success' 
AND p.direction = 'Аналитика'
AND o.buy_date >= '2021-10-01'
AND o.buy_date < '2022-01-01'
GROUP BY name
HAVING sum(order_sum) >= 1000000
ORDER BY s DESC
```

# [12] Задача 

200


# [13] Задача 

1 - выведет максимальный

# [14] Задача 

50 - в каждой группе один максимальный

# [15] Задача 
```sql
SELECT department_name
FROM department_id LEFT JOIN employee_tb USING (department_id)
GROUP BY department_name
HAVING (count distinct employee_id) > 10
ORDER BY (count distinct employee_id) DESC, department_id ASC
```

# [16] Задача 

2 3 1 2 3 4

# [17] Задача 
```sql
-- 1
select min(A), max(a), min(B), max(B) from table1 cross join table2 
-- 2
SELECT 
select(
    min(a), max(a), sum(a), count(a) FROM table1
) CROSS JOIN
SELECT (
 min(b), max(b), sum(b), count(b) FROM table2
)
```
# [18] Задача 
```sql
SELECT distinct employee_id
FROM table
WHERE date_start >= '2024-01-01' AND date_end < '2025-01-01'
GROUP BY employee_id
HAVING count(distinct date_start) >= 2
```
# [19] Задача 
```sql
SELECT count(distinct student.id) FROM student_in_class JOIN class ON student_in_class.class = class.id
WHERE class = '10B'
```
# [20] Задача 
```sql
WITH sent AS (
    SELECT DAY_TRUNC('day', date) as day, 
    user_id_sender, user_id_receiver    
    FROM table
    WHERE action = 'sent'
)
accepted AS (
    SELECT DAY_TRUNC('day', date) as day, 
    user_id_sender, user_id_receiver
    FROM table
    WHERE action = 'accepted'
)
SELECT day, count(a.user_id_receiver)/count(s.user_id_sender) * 100 as acceptance_rate
FROM sent s JOIN accepted a ON s.user_id_sender = a.user_id_sender 
AND s.user_id_receiver = a.user_id_receiver
AND s.day = a.day
GROUP BY s.day
HAVING COUNT(a.user_id_receiver) > 0
ORDER BY s.day
```

# [21] Задача 
```sql
WITH num_cnt AS (SELECT num, count(num) as cnt
FROM table
GROUP BY num)
SELECT max(num) FROM num_cnt
WHERE cnt = 1
```
# [22] Задача 
```sql
-- 1
WITH view_wid AS (SELECT date, user_client_id
FROM events
WHERE page_current_url = '/catalog'
AND action_type = 'view' AND widget_id = '522955')

click_wid AS (SELECT date, user_client_id
FROM events
WHERE page_current_url = '/catalog'
AND action_type = 'click' AND widget_id = '522955')

SELECT date, ROUND(count(distinct c.user_client_id)/count(distinct v.user_client_id), 2) * 100 as conversion_rate 
FROM
view_wid v LEFT JOIN click_wid c ON v.date = c.date AND c.user_client_id = v.user_client_id
GROUP BY date
ORDER BY date

-- 2
WITH proscroll AS (SELECT date, user_client_id
FROM events
WHERE page_current_url = '/catalog'
AND action_type = 'page_view')

view AS (SELECT date, user_client_id
FROM events
WHERE page_current_url = '/catalog'
AND action_type = 'view' AND widget_id = '522957')

SELECT date, ROUND(count(distinct v.user_client_id)/count(distinct p.user_client_id), 2) * 100 as proscroll 
FROM
proscroll p LEFT JOIN view cv ON p.date = v.date AND p.user_client_id = v.user_client_id
GROUP BY date
ORDER BY date
```

# [23] Задача 
```sql
SELECT store_id, 
AVG(total_amount) as aov,
count(distinct client_id) as clients_cnt
FROM transactions
GROUP BY store_id
```
# [24] Задача 
 1) 200
 2) 1
 3) 50

  </details>

  <details>
  <summary>[8] ROW_NUMBER [BASE]</summary>

# [1] Задача 
```sql
WITH tmp_1 AS(
    SELECT id, adr, dt,
    ROW_NUMBER() OVER (ORDER BY dt DESC) as rn
    WHERE dt <= '2023-01-01 00:00:00'
)
SELECT id, adr, dt FROM tmp_1
WHERE rn = 1
```
# [2] Задача 
```sql
WITH temp AS(
    SELECT user_id, instance_id,
    ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY date_borrowed DESC) as rn
    FROM Oplog
)
SELECT user_id, instance_id FROM remp
WHERE rn <= 3
```

# [3] Задача 
```sql
WITH sucessful AS(
    SELECT customer_id, transaction_id,amount_rur, transaction_dttm
    FROM table
    WHERE succes_flag = 'True'
)
money AS (SELECT 
customer_id
FROM sucessful
GROUP BY customer_id
HAVING sum(amount_rur) > 100000
)
rn AS(
    SELECT customer_id, transaction_id,amount_rur, transaction_dttm,
    ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY transaction_dttm ASC) as rn
    FROM money INNER JOIN sucessful USING (customer_id)
)
SELECT transaction_id, customer_id, amount_rur,transaction_dttm FROM rn
WHERE rn = 1
```
# [4] Задача 
Без каких частей оконная функция не сработает: OVER, PARTITION BY, ORDER BY?

Ответ: без OVER

# [5] Задача 
```sql
WITH temp AS(
SELECT
LEAD(name) OVER (department_id ORDER BY salary) as next_name,,
LEAD(salary) OVER (department_id ORDER BY salary) as next_salary,
ROW NUMBER() OVER (PARTITION BY department_id, salary ORDER BY id DESC) as rn
)
SELECT * FROM temp WHERE rn = 1
```
# [6] Задача 
```sql
-- 1
WITH tmp AS (SELECT user_id, program_id,
ROW_NUMBER () OVER (PARTITION BY user_id ORDER BY buy_date) as rn
FROM orders
where buy_date is not null)

SELECT user_id, name FROM tmp INNER JOIN programs p ON tmp.program_id = p.id
WHERE rn = 1

-- 2
WITH tmp AS (SELECT user_id, program_id,
ROW_NUMBER () OVER (PARTITION BY user_id ORDER BY buy_date) as rn
FROM orders
where buy_date is not null)

SELECT user_id, name FROM tmp INNER JOIN programs p ON tmp.program_id = p.id
WHERE rn = 3

-- 3
WITH tmp AS (SELECT user_id, program_id,
ROW_NUMBER () OVER (PARTITION BY user_id ORDER BY buy_date) as rn,
count(order_id) OVER (PARTITION BY user_id) as cnt
FROM orders
where buy_date is not null)

SELECT user_id, program_id,
FROM tmp
WHERE (rn = 3 AND cnt >= 3) OR (rn = 1 AND cnt < 3)
```
# [7] Задача 
```sql
-- a
WITH client_id, COALESLE(avg(sum), 0) as avg_sum
FROM 1 LEFT JOIN 2 USING (card_id)
WHERE EXTRACT (month from date) = 5
GROUP BY client_id

-- b
WITH tmp AS(
    SELECT client_id, card_id, sum(sum) as sum_total
    FROM 2 LEFT JOIN 1 USING (card_id)
    GROUP BY client_id, card_id
)
tmp_2 AS(
    SELECT client_id, card_id,
    ROW_NUMBER() OVER (PARTITION BY client_id ORDER BY sum_total DESC) as rn
    FROM tmp
)
SELECT client_id, card_id FROM tmp_2
WHERE rn = 1
```

# [8] Задача 
```sql
WITH tmp AS(
    SELECT DATE_TRUNC ('month', date) as month, category, good, sum(sum) as total_sum
    FROM table
    GROUP BY month, category, good
)
top AS(
    SELECT month, category, good, total_sum,
    RANK() OVER (PARTITION BY month, category ORDER BY total_sum DESC) as r
    FROM tmp
)

SELECT month, category, good, total_sum
FROM top
WHERE r <= 5
ORDER BY month, category, r
```

# [9] Задача 
```sql
WITH tmp AS(
SELECT *,
ROW_NUMBER() OVER (PARTITION BY id ORDER BY raw_dt DESC) as rn
FROM abnt
)
SELECT * FROM tmp 
WHERE rn = 1
```
# [10] Задача 
```sql
WITH tmp AS(
SELECT user_id, order_id
ROW_NUMBER() OVER (PARTITION BY order_id ORDER BY order_timespent DESC) as rn
FROM abnt
)
SELECT user_id, order_id  FROM tmp 
WHERE rn = 1
```

# [11] Задача 
```sql
WITH tmp AS(
    SELECT t.dt as date_trx, t.currency, c.cur_rate, t.trx,
    ROW_NUMBER() OVER (PARTITION BY t.dt, t.currency ORDER BY c.dt DESC) as rn
    FROM trx_tb1 t JOIN cur_tb1 c ON t.currency = c.currency AND c.dt <= t.dt
)
SELECT date_trx, SUM(trx*cur_rate) as sum
WHERE rn = 1
GROUP BY date_trx
ORDER BY date_trx
```

</details>

<details>
  <summary>[9] RANK / DENSE_RANK [BASE]</summary>

# [1] Задача 
```sql
SELECT
    p.ProductName,
    COUNT(o.OrderID) as amount,
    SUM(p.Price) as total,
    DENSE_RANK() OVER (ORDER BY COUNT(o.OrderID) DESC) as rank
FROM Orders o
LEFT JOIN Products p ON o.ProductID = p.ProductID
GROUP BY p.ProductName, p.ProductID
ORDER BY amount DESC
```

# [2] Задача 
```sql
WITH tmp AS (EMPL_FIO, EMPL_DEP,
DENSE_RANK () OVER (PARTITION BY EMPL_DEP ORDER BY AMOUNT DESC) as dr
FROM table
WHERE DATE_TRUNC('month', VALUE_DAY) = ‘2023-04-01’)
SELECT EMPL_FIO, EMPL_DEP FROM tmp
WHERE dr = 2
```

# [3] Задача 
```sql
WITH tmp AS (col,
DENSE_RANK () OVER (ORDER BY col DESC) as dr
FROM table)
SELECT col FROM tmp
WHERE dr = 2
```
# [4] Задача 
```sql
WITH tmp AS (SELECT EMPL_FIO,
DENSE_RANK () OVER (ORDER BY salary DESC) as dr,
ROW_NUMBER() OVER (PARTITION BY EMPL_FIO ORDER BY age) as rn
FROM table
)
SELECT EMPL_FIO FROM tmp
WHERE dr = 2 AND rn = 1
```
# [5] Задача 
```sql
WITH tm AS(
    program_id, count(order_id) as cnt
    FROM programs LEFT JOIN orders USING (program_id)
    WHERE buy_date >= DATE_TRUNC('month', current_date)
    AND state = 'success' 
)

tmp AS (SELECT program_id,
RANK () OVER (ORDER BY cnt DESC) as dr,
FROM table
)
SELECT program_id FROM tmp
WHERE dr <= 5
```

# [6] Задача 
```sql
WITH tmp AS (col,
DENSE_RANK () OVER (ORDER BY col DESC) as dr
FROM table)
SELECT col FROM tmp
WHERE dr = 2
```

</details>