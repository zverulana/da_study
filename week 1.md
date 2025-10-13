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

# [5] Задача - в процессе
    ```sql
WITH min_sal AS(
    SELECT *,
    DENSE_RANK () OVER (ORDER BY amount) as rank
    FROM salary
    WHERE VALUE_DAY between '2023-05-01' and '2023-05-31'
),
tmp AS(
SELECT *,
amount - LAG(amount, 1, 0) OVER (PARTITION BY EMPL_FIO, EMPL_DEP ORDER BY VALUE_DAY) as diff)

SELECT EMPL_FIO, diff
FROM min_sal LEFT JOIN tmp ON min_sal.EMPL_FIO = tmp.EMPL_FIO AND min_sal.EMPL_DEP = tmp.EMPL_DEP
WHERE rank <= 3
```
 </details>
 