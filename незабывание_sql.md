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

</details>