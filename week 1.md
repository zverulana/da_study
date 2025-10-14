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



</details>