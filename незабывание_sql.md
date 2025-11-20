<details>
  <summary>16 - 22 октября</summary>

# [1] Задача (16 октября)
 ```sql
-- Найдите топ-3 сотрудников в каждом отделе по зарплате, выведите их имена, отделы и ранг. Исключите сотрудников с зарплатой ниже 30000

with tmp as (select first_name, last_name, department_name, salary,
DENSE_RANK() over (partition by department_name order by salary desc) as rank
from sandbox.departments_zvereva d inner join sandbox.employees_zvereva e ON d.department_id = e.department_id and salary > 30000)

select first_name, last_name, department_name, salary from tmp
where rank <= 3
 ```
# [2] Задача (17 октября)

 ```sql
with tmp as (select department_id, department_name,
AVG(salary) as average
from sandbox.employees_zvereva e
inner join sandbox.departments_zvereva d using (department_id)
group by department_name,department_id)

select first_name, last_name, department_name, s.salary -  t.average as diff 
from sandbox.employees_zvereva s inner join tmp t on s.department_id = t.department_id 
and s.salary > t.average
order by diff desc
 ```
# [3] Задача (18 октября)

 ```sql
-- Для каждого отдела найдите сотрудника с самой высокой зарплатой и выведите его имя, отдел и зарплату. Исключите отделы с менее чем 3 сотрудниками

with tmp AS(
select first_name, last_name, department_name, salary,
rank() over (partition by department_id order by salary) as rank,
count(e.id) over (partition by department_id) as cnt
from sandbox.employees_zvereva e inner join sandbox.departments_zvereva d using (department_id)
)

select  first_name, last_name, department_name, salary from tmp
where rank = 1 and cnt >= 3

 ```
# [4] Задача (19 октября)

  ```sql
-- Выведите клиентов и их заказы, добавив ранг заказов по сумме для каждого клиента. Показать только заказы с суммой > 1000

select customer_name, order_id,
rank () over (partition by customer_id order by amount) as rank
from sandbox.customers_zvereva c join sandbox.orders_zvereva o using (customer_id)
where amount > 1000

 ```

 # [5] Задача (20 октября)

  ```sql
-- Выведите клиентов и их заказы, добавив ранг заказов по сумме для каждого клиента. Показать только заказы с суммой > 1000

select customer_name, order_id,
rank () over (partition by customer_id order by amount) as rank
from sandbox.customers_zvereva c join sandbox.orders_zvereva o using (customer_id)
where amount > 1000

 ```

  # [6] Задача (21 октября)

  ```sql
-- Выведите сумму заказов по клиентам и их долю от общей суммы всех заказов, но только для клиентов с более чем 2 заказами

with tmp as (
    select 
        customer_name, 
        sum(amount) as total_client,
        count(order_id) as cnt
    from sandbox.customers_zvereva c 
    join sandbox.orders_zvereva o using (customer_id)
    group by customer_name
)

select 
    customer_name, 
    total_client,
    ROUND(total_client / (select sum(amount) from sandbox.orders_zvereva) * 100,2) as rate
from tmp
where cnt > 2

 ```
  # [7] Задача (22 октября)

  ```sql
-- Покажите сотрудников, чья зарплата входит в топ-10% по их отделу, с указанием ранга.

with tmp as(select department_name, first_name, last_name, salary,
rank() over (partition by department_id order by salary desc) as rank,
count(id) over (partition by department_id) as cnt
from sandbox.departments_zvereva JOIN sandbox.employees_zvereva using (department_id))
select department_name, first_name, last_name
from tmp 
where rank <= CEIL(0.1 * tmp.cnt)  

 ```

</details><details> <summary>23 - 30 октября</summary>

  # [8] Задача (23 октября)

  ```sql
-- Выведите заказы с указанием, сколько дней прошло с предыдущего заказа того же клиента
 
select order_id, customer_name,
order_date - LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date) as diff
from sandbox.orders_zvereva INNER JOIN sandbox.customers_zvereva USING (customer_id)

 ```
  # [9] Задача (24 октября)

  ```sql
-- Найдите отделы, где средняя зарплата выше 50000, и выведите сотрудников с их рангом по зарплате в этих отделах
 
with tmp as(select department_name, department_id
from sandbox.departments_zvereva d join sandbox.employees_zvereva e using (department_id)
group by department_id
having avg(salary) > 50000)

select department_name, first_name, last_name,
rank() over (partition by department_name order by salary desc) as rank
from tmp join sandbox.employees_zvereva e using (department_id)

 ```
   # [10] Задача (25 октября)

  ```sql
-- Выведите клиентов, у которых есть заказы с суммой выше средней по всем заказам, и добавьте их общее количество заказов
 
select customer_id, count(order_id) as cnt from (
  select customer_id, 
  order_id
  from sandbox.orders_zvereva
  group by customer_id, order_id
  having amount > (
  select avg(amount) from sandbox.orders_zvereva)
)
group by customer_id

 ```
# [11] Задача (26 октября)

  ```sql
-- Выведите сотрудников и их зарплаты, добавив столбец с общей суммой зарплат в их отделе, но только для отделов с более чем 5 сотрудниками
 
with tmp as(select distinct department_name, department_id,
sum(salary) over (partition by department_name) as sm,
count(e.id) over (partition by department_name) as cnt
from sandbox.departments_zvereva d join sandbox.employees_zvereva e using (department_id))
select first_name, last_name, salary, sm from
sandbox.employees_zvereva join tmp using (department_id)
where cnt > 5
order by salary desc

 ```

# [12] Задача (27 октября)

  ```sql
-- Покажите заказы, где сумма заказа выше средней суммы заказов за тот же месяц, с указанием клиента
 
swith tmp as (select order_id, amount, customer_id,
avg(amount) over(partition by date_trunc('month', order_date)) as ag
from sandbox.orders_zvereva)
select order_id, customer_name 
from tmp join sandbox.customers_zvereva c ON tmp.customer_id = c.customer_id and tmp.amount > tmp.ag
 ```

 # [13] Задача (28 октября)

  ```sql
-- Выведите сотрудников с их зарплатами и кумулятивной суммой зарплат по отделу, отсортированной по дате найма
 
select first_name, last_name, salary, department_name, 
sum(salary) over (partition by department_id order by hire_date) as sm
from sandbox.departments_zvereva join sandbox.employees_zvereva using (department_id)
order by department_name, hire_date
 ```

  # [14] Задача (29 октября)

  ```sql
-- Найдите клиентов, у которых максимальная сумма заказа превышает 5000, и выведите их заказы с рангом по сумме
 
with tmp as(select customer_id, customer_name, max(amount) as mx
from sandbox.customers_zvereva join sandbox.orders_zvereva using (customer_id)
group by customer_id, customer_name
having max(amount) > 5000)

select customer_name, order_id, amount,
rank() over (partition by customer_name order by amount desc) as rank
from tmp left join sandbox.orders_zvereva using (customer_id) 
order by customer_name, rank
 ```

# [15] Задача (30 октября)

  ```sql
-- Выведите сотрудников, чья зарплата растет по сравнению с предыдущим сотрудником в том же отделе (по дате найма)
 
with tmp as (select id, first_name, last_name, department_id, salary,hire_date,
lag(id) over (partition by department_id order by hire_date) as lg
from sandbox.employees_zvereva)

select tmp.first_name, tmp.last_name, tmp.department_id, tmp.salary, tmp.hire_date
from tmp join sandbox.employees_zvereva e on tmp.lg = e.id and tmp.salary > e.salary
 ```

</details>



<details>
  <summary>31 окт - 6 ноября</summary>

# [16] Задача (31 октября)

  ```sql
-- Покажите общее количество заказов по клиентам и их долю от всех заказов, но только для заказов за 2024 год
 
select customer_name, 
count (order_id) as cnt,
round(count (order_id) * 1.0 / (
	select count(order_id) from sandbox.orders_zvereva 
	where order_date >= '2024-01-01' and order_date <= '2024-12-31'), 2) as rate
from sandbox.customers_zvereva left join sandbox.orders_zvereva using (customer_id) 
where order_date >= '2024-01-01' and order_date <= '2024-12-31'
group by customer_name

 ```

 # [17] Задача (1 ноября)

  ```sql
-- Выведите сотрудников, чья зарплата выше медианной по их отделу

with tmp as (select department_id, 
percentile_cont(0.5) within group (order by salary) as pc
from sandbox.employees_zvereva 
group by department_id)
select first_name, last_name
from sandbox.employees_zvereva e inner join tmp on e.department_id = tmp.department_id and salary > pc

 ```

  # [18] Задача (2 ноября)

  ```sql
-- Найдите заказы, где сумма превышает сумму следующего заказа того же клиента
with tmp as (select customer_id, order_id, amount,
lead(amount) over (partition by customer_id order by order_date) as ld
from sandbox.orders_zvereva)
select order_id
from tmp
where amount>ld


 ```

 # [19] Задача (3 ноября)

  ```sql
-- Выведите отделы с более чем 4 сотрудниками и их среднюю зарплату, добавив ранг отделов по средней зарплате
with tmp as(
select department_id, 
round(avg(salary),0) as ag
from sandbox.employees_zvereva
group by department_id
having count(id) > 4
)
select department_name, ag, 
rank() over (order by ag desc)
from tmp left join sandbox.departments_zvereva  using (department_id)


 ```
 # [20] Задача (4 ноября)

  ```sql
-- Покажите сотрудников, чья зарплата составляет более 20% от общей суммы зарплат в их отделе.

with tmp as(
select department_id, sum(salary) as sm
from sandbox.employees_zvereva 
group by department_id
)
select first_name, last_name, salary
from tmp inner join sandbox.employees_zvereva e  on tmp.department_id = e.department_id and
salary/sm > 0.2


 ```
 # [21] Задача (5 ноября)

  ```sql
-- Выведите клиентов и их заказы, добавив столбец с максимальной суммой заказа по клиенту. 
---Показать только заказы за последние 6 месяцев

select customer_name, 
order_id,
max(amount) over (partition by customer_name) as mx
from sandbox.orders_zvereva join sandbox.customers_zvereva using (customer_id)
where order_date >= current_date - interval '6 month'
order by mx desc, customer_name, order_id

 ```

 # [22] Задача (6 ноября)

  ```sql
-- Найдите сотрудников, чья зарплата выше средней зарплаты сотрудников, на нанятых в том же году, и выведите их отдел

with tmp as(select first_name, last_name, department_name, salary,
avg(salary) over (partition by extract(year from hire_date)) as ag
from sandbox.employees_zvereva join sandbox.departments_zvereva using (department_id))
select first_name, last_name, department_name from tmp
where salary > ag

 ```

  </details>


  <details>
  <summary>7 - 14 ноября</summary>


  # [23] Задача (7 ноября)

  ```sql
-- Выведите заказы с указанием, сколько заказов было сделано клиентом до текущего заказа
-- Мой изначальный код

with tmp as(select customer_id, order_date, order_id, count(order_id) as cnt from sandbox.orders_zvereva
group by customer_id, order_date, order_id
order by customer_id, order_date),

tmp_2 as(select customer_id, order_date,order_id,
sum(cnt) over (partition by customer_id order by order_date) as sm
from tmp)

select order_id, LAG(sm, 1, 0) over (partition by customer_id order by order_date) as lg
from tmp_2

-- Посмотрела пример

select order_id, 
count(*) over (partition by customer_id order by order_date rows between unbounded preceding and current row) -1
from sandbox.orders_zvereva

 ```
  # [24] Задача (8 ноября)

  ```sql
-- Найдите отделы, где максимальная зарплата превышает 100000, и выведите сотрудников с их долей от максимальной зарплаты

with tmp as (select department_id,department_name,  max(salary) as mx
from sandbox.employees_zvereva inner join sandbox.departments_zvereva using (department_id)
group by department_id, department_name)
select department_name, first_name, last_name,
salary/mx*100 as rate
from tmp inner join sandbox.employees_zvereva  using(department_id)
where mx > 100000

 ```

# [25] Задача (9 ноября)

  ```sql
-- Выведите сотрудников и их зарплаты, добавив столбец с разницей от средней зарплаты по всем сотрудникам, нанятым позже
with tmp as (select first_name, last_name, salary, hire_date, 
avg(salary) over (partition by department_id order by hire_date rows between 1 following and unbounded following) as diff
from sandbox.employees_zvereva
order by hire_date)
select first_name, last_name, salary, round(salary - diff,2) as diff
from tmp

 ```

 # [26] Задача (10 ноября)

  ```sql
-- Покажите клиентов с их общим количеством заказов и рангом по количеству заказов, но только для клиентов с суммой заказов > 10000

with tmp as (select customer_name, 
count(order_id) as cnt,
sum(amount) as sm
from sandbox.customers_zvereva left join sandbox.orders_zvereva using (customer_id)
group by customer_name)
select customer_name, 
rank() over (order by sm desc)
from tmp
where sm > 10000

 ```

  # [27] Задача (11 ноября)

  ```sql
-- Выведите заказы, где сумма заказа выше средней суммы предыдущих заказов клиента

with tmp as (select customer_id, order_id, amount,
avg(amount) over (partition by customer_id order by order_date rows between UNBOUNDED PRECEDING and 1 PRECEDING) as ag
from sandbox.orders_zvereva
order by customer_id, order_date)
select customer_id, order_id
from tmp
where amount > ag

 ```
# [28] Задача (12 ноября)

  ```sql
-- Найдите сотрудников, чья зарплата входит в нижние 25% по отделу, и выведите их отдел

with tmp as (select department_id,
percentile_cont(0.25) within group (order by salary) as q
from sandbox.employees_zvereva
group by department_id)
select department_name, first_name, last_name, salary
from sandbox.employees_zvereva e inner join  sandbox.departments_zvereva d on e.department_id=d.department_id 
join tmp on e.department_id = tmp.department_id and e.salary <= tmp.q
order by department_name


 ```

 # [29] Задача (13 ноября)

  ```sql
-- Выведите клиентов, у которых средняя сумма заказа выше средней суммы всех заказов, и их общее количество заказов
with tmp as (select customer_name,
avg(amount) over (partition by customer_id) as ag,
count(order_id) over (partition by customer_id) as cnt
from sandbox.orders_zvereva join sandbox.customers_zvereva using (customer_id))
select customer_name, ag, cnt
from tmp
where ag > (select avg(amount) from sandbox.orders_zvereva)
group by customer_name, ag, cnt

 ```

</details>



  <details>
  <summary>14 - 20 ноября</summary>

# [30] Задача (14 ноября)

  ```sql
-- Покажите сотрудников, нанятых в первые 10% по дате найма в их отделе, и выведите их зарплату

with tmp as(select first_name, last_name, department_id, salary, hire_date,
row_number() over (partition by department_id order by hire_date) as rn,
count(*) over (partition by department_id) as cnt
from sandbox.employees_zvereva)
select first_name, last_name, department_id, salary, hire_date, rn, cnt
from tmp
where rn <= ceil(cnt*0.1)

 ```

 # [31] Задача (15 ноября)

  ```sql
-- Выведите заказы, где сумма заказа составляет более 50% от максимальной суммы заказа клиента.

with tmp as (select customer_id, max(amount) as mx
from sandbox.orders_zvereva
group by customer_id)
select customer_name, order_id, amount
from tmp join sandbox.orders_zvereva using (customer_id) join sandbox.customers_zvereva using (customer_id) 
where amount > mx/2

 ```
# [32] Задача (16 ноября)

  ```sql
-- Найдите отделы, где минимальная зарплата выше 20000, и выведите сотрудников с их рангом по зарплате

with tmp as(select department_id, min(salary) as mn
from sandbox.employees_zvereva
group by department_id)
select department_name, first_name, last_name, 
rank() over (partition by tmp.department_id order by salary desc) as rank
from tmp left join sandbox.employees_zvereva e on tmp.department_id = e.department_id 
inner join sandbox.departments_zvereva d on tmp.department_id = d.department_id and tmp.mn > 20000 

 ```

# [33] Задача (17 ноября)

  ```sql
-- Выведите клиентов и их заказы, добавив столбец с количеством заказов клиента за тот же месяц

select customer_name, order_id,
count(order_id) over (partition by customer_id, date_trunc('month', order_date)) as cnt
from sandbox.orders_zvereva join sandbox.customers_zvereva using (customer_id)
order by customer_name, order_id, cnt

 ```

 # [34] Задача (18 ноября)

  ```sql
-- Покажите сотрудников, чья зарплата ниже средней зарплаты сотрудников, нанятых раньше, и выведите их отдел

with tmp as (select id, first_name, last_name, department_id, salary,
avg(salary) over (partition by department_id order by hire_date rows between unbounded preceding and 1 preceding) as ag
from sandbox.employees_zvereva 
order by department_id, hire_date)
select first_name, last_name, department_id, salary
from tmp
where salary < ag

 ```

 # [35] Задача (19 ноября)

  ```sql
-- Выведите сотрудников, чья зарплата выше 75-го процентиля зарплат в их отделе

with tmp as (select department_id,
percentile_cont (0.75) within group (order by salary) as pc
from sandbox.employees_zvereva
group by department_id)

select first_name,  last_name, department_name
from sandbox.employees_zvereva e 
join tmp t on e.department_id = t.department_id and e.salary >= t.pc 
join sandbox.departments_zvereva d on e.department_id = d.department_id
order by e.department_id, salary

 ```

 # [36] Задача (20 ноября)

  ```sql
-- Найдите заказы, сделанные в последний день месяца, и выведите их сумму и клиента
select order_id, customer_name, amount
from sandbox.orders_zvereva o join sandbox.customers_zvereva c using (customer_id)
where order_date = date_trunc('month', order_date) + interval '1 month - 1 day'


 ```