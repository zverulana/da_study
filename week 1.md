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
ORDER BY dt
```
  # [2] Задача


 </details>
 
  